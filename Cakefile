fs = require 'fs'

{print} = require 'sys'
{spawn} = require 'child_process'

build = (callback) ->
  coffee = spawn 'coffee', ['-c', '-o', 'lib', 'src']
  coffee.stderr.on 'data', (data) ->
    process.stderr.write data.toString()
  coffee.stdout.on 'data', (data) ->
    print data.toString()
  coffee.on 'exit', (code) ->
    callback?() if code is 0

task 'build', 'Build lib/ from src/', ->
  build()




  # Build to compile in a single call from the terminal
# build = (callback) ->
#   coffee = spawn 'coffee', ['-c', '-o', 'lib', 'src']
#   coffee.stderr.on 'data', (data) ->
#     process.stderr.write data.toString()
#   coffee.stdout.on 'data', (data) ->
#     print data.toString()
#   coffee.on 'exit', (code) ->
#     callback?() if code is 0
# task 'build', 'Build lib/ from src/', ->
#   build()



# Watch in the terminal to continuously watch for changes in the src directory
 task 'watch', 'Watch src/ for changes', ->
    coffee = spawn 'coffee', ['-w', '-c', '-o', 'lib', 'src']
    coffee.stderr.on 'data', (data) ->
      process.stderr.write data.toString()
    coffee.stdout.on 'data', (data) ->
      print data.toString()

# TRYING: You could change 'lib' to 'js' or whatever output dir you want, but
# how about trying the options like the method used in `build`
task 'watch:options', 'Watch src/ directory for changes and now with options for output dir', (options) ->
  # TRYING: `options` object instead and pass a different output dir
  coffee = spawn 'coffee', ['-w', '-c', '-o', options.output or 'lib', 'src']
  coffee.stderr.on 'data', (data) ->
    process.stderr.write data.toString()
  coffee.stdout.on 'data', (data) ->
    print data.toString()


# Bare Watch in the terminal to continuously watch for changes in the src directory
task 'watch:bare', 'Watch src/ directory for changes', ->
  coffee = spawn 'coffee', ['-w', '-b', '-o', 'lib', 'src']
  coffee.stderr.on 'data', (data) ->
    process.stderr.write data.toString()
  coffee.stdout.on 'data', (data) ->
    print data.toString()



# Open the index and then start watching the source
task 'open', 'Open index.html', ->
  # First open, then watch
  spawn 'open', 'index.html'
  invoke 'watch'

# You can use options for a task, which take a short name, long name, and description
option '-o', '--output [DIR]', 'output directory'

task 'build', 'Build lib/ from src/ with optional directory', (options) ->
  # Now we have access to a `options` object
  coffee = spawn 'coffee', ['-c', '-o', options.output or 'lib', 'src']
  coffee.stderr.on 'data', (data) ->
    process.stderr.write data.toString()
  coffee.stdout.on 'data', (data) ->
    print data.toString()



# Tasks in Cakefiles can take options. This is important if you want to be able to provide parameters such as the target environment (e.g., production or development), source or target directory, etc. For more information, check out CoffeeScript's own Cakefile. If you rely on options, make sure you always have sensible defaults in place:

option '-e', '--environment [ENVIRONMENT_NAME]', 'set the environment for `task:withDefaults`'
task 'task:withDefaults', 'Description of task', (options) ->
  options.environment or= 'production'

# Then you can execute the task with your defined options:
# cake -e "myEnv" task:withDefaults

