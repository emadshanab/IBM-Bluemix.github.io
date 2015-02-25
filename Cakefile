# Licensed under the Apache License. See footer for details.

require "cakex"

toml = require "toml"

#-------------------------------------------------------------------------------
task "build", "build from source",          -> taskBuild()
task "watch", "watch for changes, rebuild", -> taskWatch()
task "bower", "get files from bower",       -> taskBower()

WatchSpec = "index-template.html *.toml"

#-------------------------------------------------------------------------------
taskBuild = ->
  taskBower()

  indexHtml = cat "index-template.html"

  people = cat "people.toml"
  people = toml.parse people

  # build people
  output = []
  output.push "<table class='table'>"

  for github, {name, image, twitter, info} of people
    continue  unless name?
    continue  unless image?
    continue  unless twitter?
    info = "" unless info?

    output.push ""
    output.push "<tr>"
    output.push "<td><img src='#{image}' width=120>"
    output.push "<td><p><a href='https://github.com/#{github}'>#{name}</a> -"
    output.push "  <a href='https://twitter.com/#{twitter}'>@#{twitter}</a>"
    output.push "  #{info}"

  output.push ""
  output.push "</table>"

  output = output.join "\n"

  indexHtml = indexHtml.replace("%%people%%", output)

  # build projects

  projects = cat "projects.toml"
  projects = toml.parse projects

  output = []
  output.push "<table class='table'>"

  for name, {description} of projects
    description = "no description!" unless description?
    output.push ""
    output.push "<tr>"
    output.push "<td><a href='https://github.com/IBM-Bluemix/#{name}'>#{name}</a>"
    output.push "<td>#{description}"

  output.push ""
  output.push "</table>"

  output = output.join "\n"

  indexHtml = indexHtml.replace("%%projects%%", output)

  indexHtml.to "index.html"
  log "generated: index.html"

#-------------------------------------------------------------------------------
taskWatch = ->
  watchIter()

  watch
    files: WatchSpec.split " "
    run:   watchIter

  watch
    files: "Cakefile"
    run: (file) ->
      return unless file == "Cakefile"
      log "Cakefile changed, exiting"
      exit 0

#-------------------------------------------------------------------------------
watchIter = ->
  taskBuild()

#-------------------------------------------------------------------------------
taskBower = ->
  copyBowerFiles "bower"

#-------------------------------------------------------------------------------
copyBowerFiles = (dir) ->

  bowerConfig = require "./bower-config"

  cleanDir dir

  for name, {version, files} of bowerConfig
    unless test "-d", "bower_components/#{name}"
      exec "bower install #{name}##{version}"
      log ""

  for name, {version, files} of bowerConfig
    for src, dst of files
      src = "bower_components/#{name}/#{src}"

      if dst is "."
        dst = "#{dir}/#{name}"
      else
        dst = "#{dir}/#{name}/#{dst}"

      mkdir "-p", dst

      cp "-R", src, dst

#-------------------------------------------------------------------------------
cleanDir = (dir) ->
  mkdir "-p", dir
  rm "-rf", "#{dir}/*"

#-------------------------------------------------------------------------------
# Copyright IBM Corp. 2014
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#-------------------------------------------------------------------------------
