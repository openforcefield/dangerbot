require 'open-uri'
require 'yaml'

message("Hello, this worked")
reviewers = YAML.load(open('https://raw.githubusercontent.com/openforcefield/dangerbot/master/reviewers.yml').read)
message(reviewers.inspect)

message(reviewers.sample)