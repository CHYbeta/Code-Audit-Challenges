# Challenge 
```ruby 
require "sqlite3"
require "webrick"

PORT = ARGV[0]

class SecureDatastore
  include Singleton

  def initialize
    @db = SQLite3::Database.new("secure.db")
  end

  def secure_species_lookup(insecure_codename)
    # roll our own escaping to prevent SQL injection attacks
    secure_codename = insecure_codename.gsub("'", Regexp.escape("\\'"))
    query = "SELECT species FROM operatives WHERE codename = '#{secure_codename}';"

    puts query
    results = @db.execute(query)

    return if results.length == 0
    results[0][0]
  end
end

server = WEBrick::HTTPServer.new(Port: PORT)

trap("INT") { server.shutdown }

class AgentLookupServlet < WEBrick::HTTPServlet::AbstractServlet
  def do_GET(request, response)
    response.status = 200
    response["Content-Type"] = "text/plain"

    response.body = SecureDatastore.instance.secure_species_lookup(request.query["codename"]) + "\n"
  end
end

server.mount "/agent_lookup", AgentLookupServlet

server.start
```

# Refference  
+ Square CTF 2017 Little Doggy Tables 
