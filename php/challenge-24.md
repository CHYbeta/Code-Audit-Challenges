# Challenge
```php 
<?php
//
// Trigger Moved to index.php
//if (false !== ($who = Common::getGet('vote_for'))) {
//      noesc_voteup($who);//}
//
/**
 * Get the database link
 * @return GDO_Database */
function noesc_db()
{
        static $noescdb = true;
        if ($noescdb === true)        {
                $noescdb = gdo_db_instance('localhost', NO_ESCAPE_USER, NO_ESCAPE_PW, NO_ESCAPE_DB);
                $noescdb->setLogging(false);
                $noescdb->setEMailOnError(false);
        }        return $noescdb;
}
 
/**
 * Create table (called by install-script) * The table layout is crappy, there is only 1 row in the table Oo.
 * @return boolean
 */
function noesc_createTable()
{       
        $db = noesc_db();
        $query =
                "CREATE TABLE IF NOT EXISTS noescvotes ( ".
                "id     INT(11) UNSIGNED PRIMARY KEY, ". # I could have one row per candidate, but currently there is only one global row(id:1). I know it`s a bit unrealistic, but at least it is safe, isn`t it?
                "bill   INT(11) UNSIGNED NOT NULL DEFAULT 0, ". # bill column                "barack INT(11) UNSIGNED NOT NULL DEFAULT 0, ". # barack column
                "george INT(11) UNSIGNED NOT NULL DEFAULT 0 )"; # george columb
        
        if (false === $db->queryWrite($query)) {
                return false;        
        }
        return noesc_resetVotes();
}
 
/** * Reset the votes.
 * @return void
 */
function noesc_resetVotes()
{       noesc_db()->queryWrite("REPLACE INTO noescvotes VALUES (1, 0, 0, 0)");
        echo GWF_HTML::message('No Escape', 'All votes have been reset', false);
}
 
/** * Count a vote.
 * Reset votes when we hit 100 or 111.
 * TODO: Implement multi language
 * @param string $who
 * @return void */
function noesc_voteup($who)
{
        if ( (stripos($who, 'id') !== false) || (strpos($who, '/') !== false) ) {
                echo GWF_HTML::error('No Escape', 'Please do not mess with the id. It would break the challenge for others', false);                return;
        }
 
 
        $db = noesc_db();        
        $who = mysql_real_escape_string($who);
        $query = "UPDATE noescvotes SET `$who`=`$who`+1 WHERE id=1";
        if (false !== $db->queryWrite($query)) {
                echo GWF_HTML::message('No Escape', 'Vote counted for '.GWF_HTML::display($who), false);
        }        
        noesc_stop100();
}
 
/** * Get all votes.
 * @return array
 */
function noesc_getVotes()
{        return noesc_db()->queryFirst("SELECT * FROM noescvotes WHERE id=1");
}
 
/**
 * Reset when we hit 100. Or call challenge solved on 111. * @return void
 */
function noesc_stop100()
{
        $votes = noesc_getVotes();        
        foreach ($votes as $who => $count)
        {
                if ($count == 111) {
                        noesc_solved();
                        noesc_resetVotes();                        
                        break;
                }
                
                if ($count >= 100) {
                        noesc_resetVotes();                        
                        break;
                }
        }
}
 /**
 * Display fancy votes table.
 * New: it is multi language now.
 * @return unknown_type
 */
function noesc_displayVotes(WC_Challenge $chall)
{
        $votes = noesc_getVotes();
        echo '<table>';
        echo sprintf('<tr><th>%s</th><th>%s</th><th>%s!</th></tr>', $chall->lang('th_name'), $chall->lang('th_count'), $chall->lang('th_vote'));        $maxwho = '';
        $max = 0;
        $maxcount = 0;
        // Print Candidate rows
        foreach ($votes as $who => $count)        {
                if ($who !== 'id') // Skip ID
                {
                        $count = (int) $count;
                        if ($count > $max) {                                
                                $max = $count;
                                $maxwho = $who;
                                $maxcount = 1;
                        }
                        elseif ($count === $max) {                                
                                $maxcount++;
                        }
                        $button = GWF_Button::generic($chall->lang('btn_vote', array($who)), "index.php?vote_for=$who");
                        echo sprintf('<tr><td>%s</td><td class="gwf_num">%s</td><td>%s</td></tr>', $who, $count, $button);
                }        
        } 
        echo '</table>';
 
        // Print best candidate.        
        if ($maxcount === 1) {                
                echo GWF_Box::box($chall->lang('info_best', array(htmlspecialchars($maxwho))));
        }
}
 
/** * Try to get here :)
 */
function noesc_solved()
{
        if (false === ($chall = WC_Challenge::getByTitle('No Escape'))) {          
                $chall = WC_Challenge::dummyChallenge('No Escape', 2, '/challenge/no_escape/index.php', false);
        }
        $chall->onChallengeSolved(GWF_Session::getUserID());
}
 ?>
```

# Solution 
流程大概如下：
1. 点击vote按钮后，调用noesc_voteup()
2. noesc_voteup()中，执行update操作后，调用noesc_stop100()
3. noesc_stop100()，先检查票数是否为111，若是则通过。接着检查票数是否大于等于100，若是则清零。

也就是说，通过vote的方法是达不到票数为111的。看一下noesc_voteup()的第60行左右：
```php
$who = mysql_real_escape_string($who);
$query = "UPDATE noescvotes SET `$who`=`$who`+1 WHERE id=1";
```
mysql_real_escape_string()会对如下字符进行转义，即在前面加上反斜杠：
+ \x00
+ \n
+ \r
+ \\
+ '
+ "
+ \x1a
该函数在php5.5.0后废弃，php7.0.0开始移除。

在本题中，update的操作是用反引号来包含，所以如果我们传入参数为 ``bill`=111 -- +``
显示的sql语句为：
```
UPDATE noescvotes SET `bill`=111 -- `=`bill`=111 -- `+1 WHERE id=1
```
将bill票数设置为111，并且通过`-- `使得后面注释掉。从而成功注入。

访问：https://www.wechall.net/challenge/no_escape/index.php
?vote_for=bill`=111 -- +

# Refference 
+ [WeChall：No escape](https://www.wechall.net/challenge/no_escape/index.php)


