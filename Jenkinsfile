#!/usr/bin/env groovy
 

 
import hudson.model.*
import hudson.EnvVars
import groovy.json.JsonSlurperClassic
import groovy.json.JsonBuilder
import groovy.json.JsonOutput
import java.net.URL
 

node {
stage '\u2776 Stage 1'
echo "\u2600 BUILD_URL=${env.BUILD_URL}"
 
def workspace = pwd()
echo "\u2600 workspace=${workspace}"
 
stage '\u2777 Stage 2'

   wrap([$class: 'TimestamperBuildWrapper']) {
      echo "Done"
   }

} // node

