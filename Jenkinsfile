#!/usr/bin/env groovy
 

 
import hudson.model.*
import hudson.EnvVars
import groovy.json.JsonSlurperClassic
import groovy.json.JsonBuilder
import groovy.json.JsonOutput
import java.net.URL
 

node {
stage '\u2776 Stage 1'
git url: "https://github.com/MNT-Lab/mntlab-pipeline.git", branch: 'atsuranau'

echo "\u2600 Repo downloaded"
 

 
stage '\u2777 Stage 2'

   wrap([$class: 'TimestamperBuildWrapper']) {
      echo "Done"
   }

} // node

