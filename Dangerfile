require 'open-uri'
require 'yaml'



# The following is taken from GitLab's reviewer roulette:
# https://gitlab.com/gitlab-org/gitlab-foss/blob/master/danger/roulette/Dangerfile
# There is a MUCH more sophisticated system there, which we might begin
# adopting more of if the need arises.

MESSAGE = <<MARKDOWN
## Reviewer roulette

Changes that require review have been detected!
MARKDOWN

CATEGORY_TABLE_HEADER = <<MARKDOWN

To spread load more evenly across eligible reviewers, Danger has randomly picked
a candidate for this review. Feel free to override this selection if you
think someone else would be better-suited, or the chosen person is unavailable.
This bot will not run if the string "#trivial" appears in the PR name or body
text, the string "[WIP]" appears in the PR title, or if a reviewer is already assigned.

Once you've decided who will review this merge request, mention them as you
normally would! Danger does not (yet?) automatically notify them for you.

| Reviewer |
|----------|
MARKDOWN


pp github

client = github.api

repo = github.full_name # [":repo"][":full_name"]
number = github.number # [":number"]

pr = client.pull_request(repo, number)
comments = client.issue_comments(repo, number)


dangerbot_assigned = false
dangerbot_already_posted = false
for comment in comments
  if comment[":user"][":login"] == "openff-dangerbot"
    dangerbot_already_posted = true
  end
end

pp pr["assignees"]
for assignee in pr["assignees"]
  #puts "|"+assignee["login"]|
  pp assignee[":login"]
  if assignee[":login"] == "openff-dangerbot"
    #if assignee[":login"].include?("off-dangerbot")
    #puts "aaaa"
    dangerbot_assigned = true
  end
end


puts "dangerbot assigned?  " + dangerbot_assigned.to_s
puts "dangerbot already posted? " + dangerbot_already_posted.to_s


#if !(github.pr_title + github.pr_body).include?("#trivial") and !github.pr_title.include?("[WIP]") and (github.pr_json["requested_reviewers"] == nil)

if dangerbot_assigned and not(dangerbot_already_posted)
  reviewers = YAML.load(open('https://raw.githubusercontent.com/openforcefield/dangerbot/master/reviewers.yml'))
  markdown(MESSAGE)
  markdown(CATEGORY_TABLE_HEADER + "| @j-wags |\n| " + reviewers.sample + " |")
  markdown("This reviewer was selected out of a list of Open Force Field volunteers: "+ reviewers.inspect)
  client.update_issue(repo, number, {assignee: "j-wags"})
end


#client = github.api



#comments = client.issue_comment("openforcefield/openforcefield", 490)

#pp comments

#dangerbot_review_requested = github.pr_json["reviewers"]
#dangerbot_already_posted = false



#for comment in comments
#  if comment[":user"][":login"] == "openff-dangerbot"
#    dangerbot_already_posted = true
#  end
#end



#if comment[":body"].downcase.include?("@openff-dangerbot Ready for review")
#if github.
#roulette_spin_requested = true
#end




#message(github.pr_json["assignee"])

#require "pp"
#pp github.api.pull_requests.pull_request_comments("openforcefield/openforcefield", 490)
#message(github.pr_json.to_json)
#puts "\n\n\n\n\n"
#pp github.pr_json

#data = github.pr_json
#data.sort{|a,b| a[1]<=>b[1]}.each { |elem|
#  message("#{elem[1]}, #{elem[0]}")
#}


  # reviewer = reviewers.sample
  # message(reviewers.inspect)

# message(reviewers.sample)
# puts(github.pr_json["assignee"])

