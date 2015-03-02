# # from the Cakefile
# # Watch in the terminal to continuously watch for changes in the src directory
#  task 'watch', 'Watch src/ for changes', ->
#     coffee = spawn 'coffee', ['-w', '-c', '-o', 'lib', 'src']
#     coffee.stderr.on 'data', (data) ->
#       process.stderr.write data.toString()
#     coffee.stdout.on 'data', (data) ->
#       print data.toString()


desc "compile and run the site"
task :default do
  pids = [
    spawn("sass --watch stylesheets/sass:stylesheets"),
    spawn("coffee -b -w -o js -c src/*.coffee")
  ]

  trap "INT" do
    Process.kill "INT", *pids
    exit 1
  end

  loop do
    sleep 1
  end
end


# FROM NICK Q's orginal post:
# http://quaran.to/blog/2013/01/09/use-jekyll-scss-coffeescript-without-plugins/

# desc "compile and run the site"
# task :default do
#   pids = [
#     spawn("jekyll"), # put `auto: true` in your _config.yml
#     spawn("scss --watch assets:stylesheets"),
#     spawn("coffee -b -w -o javascripts -c assets/*.coffee")
#   ]

#   trap "INT" do
#     Process.kill "INT", *pids
#     exit 1
#   end

#   loop do
#     sleep 1
#   end
# end

# This Rakefile comes from the Jekyll orderofeverything.com modified to have the Jekyll bit removed.
# Originally from Nick Quaranto through: http://quaran.to/blog/2013/01/09/use-jekyll-scss-coffeescript-without-plugins/

# desc "watch coffee and sass files"
# task :default do
#   pids = [
#     spawn("compass watch"), # -s compressed
#     spawn("coffee -b -w -o javascripts -c src/*.coffee")
#   ]

#   trap "INT" do
#     Process.kill "INT", *pids
#     exit 1
#   end

#   loop do
#     sleep 1
#   end
# end



