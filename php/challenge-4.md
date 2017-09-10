# Challenge 
```php 
if file_src == "vpn_logo_upload":
    data = request.files.vpn_logo
    filename = data.filename
    if data.file:
        file_ext = os.path.splitext(filename)[1]
        output_path = "/usr/vtm/var/www/html/vpn/upload/" + "vpn_logo" + file_ext
        bak_tag = False
        bak_file_path = output_path + ".bak"
        if os.path.exists(output_path):
            cmd = "mv -f " + output_path + " " + bak_file_path
            os.system(cmd)
            bak_tag = True
        write_file(filename, data.file, output_path)
        file_size = os.path.getsize(output_path)
        file_type = mimetypes.guess_type(output_path)
        del_cmd = "rm -f " + output_path
        if file_type[0] != "image/jpeg" and file_type[0] != "image/png" and file_type[0] != "image/gif":
            result = {"return": -2, "reason": file_type[0]}
            os.system(del_cmd)
        elif file_size < file_size_1M:
            result["data"]["new_name"] = "vpn_logo" + file_ext
        else:
            result = {"return": -2, "reason": "file is too large"}
            os.system(del_cmd)
            if bak_tag:
                bak_cmd = "mv -f " + bak_file_path + " " + output_path
                os.system(bak_cmd)
```


# Refference 
+ [l3m0n:小密圈专题(2)-命令执行绕过](http://www.cnblogs.com/iamstudy/articles/command_exec_tips_1.html)

