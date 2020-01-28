require 'open-uri'
require 'yaml'



# The following is taken from GitLab's reviewer roulette:
# https://gitlab.com/gitlab-org/gitlab-foss/blob/master/danger/roulette/Dangerfile
# There is a MUCH more sophisticated system there, which we might begin
# adopting more of if the need arises.

PART_ONE = <<MARKDOWN
## Reviewer roulette

A review has been requested! 

To spread load more evenly across eligible reviewers, Danger has randomly picked \
a candidate for this review and assigned them to this PR.

If you need to run the roulette again (for example, if the assigned reviewer \
is unavailable), simply un-request a review from me, then request again. \
After about 45 seconds, I will update the message with a new random reviewer.

| Reviewer |
|----------|
MARKDOWN

PART_TWO = <<MARKDOWN

## Review guidelines

_Some notes from @j-wags that we can refine as we do this more_

### Timeline and responsibilities

The PR reviewer should perform a review within 48 hours of being assigned.

The review may take up to three hours

If few or insignificant changes are needed, the reviewer should accept the PR.

If substantial fixes should be made (see categories below), the reviewer \
should request changes, indicating which comments are high-priority (blocking), \
as opposed to questions or comments.

The PR author will then correct any blocking issues and re-request review. \
The re-review should focus just on the changes that were requested and any \
new code that was added in response.

The PR author is the only person who should modify the code in the branch, and it \
is customary to let them press the "merge" button once the PR is approved. Either \
person can "resolve" non-blocking comment threads, but only the reviewer should "resolve" \
comment threads that prompt a re-review.

### The PR author

The person requesting the review should ensure that the purpose of the review \
is clearly explained, by linking relevant GitHub Issues in the PR body \
text (ex "Closes #12"), using clear variable names, commenting \
non-obvious code, and identifying areas of the diff that are unusual \
(ex. "the molecule_to_string function was cut and pasted to a different \
file and didn't change, so don't review it"). If the PR diff is larger than 300 lines, they \
should identify the area to prioritize for the review, to let the reviewer \
add as much value as possible if they are time-constrained.

### The PR assignee

The newly-assigned reviewer should acknowledge that they recieved the request, \
and confirm that they can perform the review within 48 hours. Generally, a \
good review strategy is to:

* Ensure that you understand the overall purpose of the codebase  \
  (consider looking at the codebase's tests or examples \
  to understand how the functions are used)
* Read through the body text of the PR (click through to any relevant \
  tagged issues to understand the discussion around the changes)
* Ensure that you understand the purpose of the changes in the PR
* Read over the entire PR diff (minus data files) without commenting
* Then, begin adding comments or thoughts, in the order of
  * Examples
  * Tests
  * Core code

### Types of comments

I've found that my comments fall into a few rough categories. This is a list of
them in descending order of value:

[If any of these are present, you should request changes]

* Conceptual -- "I don't understand what these changes aim to do"
* Scientific -- "This won't work because nitrogen can have four bonds"
* Algorithmic -- "This may crash because the input list is empty"
* Testing -- "This functionality was added, but never tested". If the codebase \
  is very young, this may be waived, but after a few months, test coverage should \
  strictly increase with each PR.

[These may be blocking at the reviewer's discretion]
* API -- "The name of this function is confusing, and I'd expect it \
  to do something else"
* Documentation -- "Add docstrings to these functions/Improve the ones \
  that are there"

[Simple fixes that generally aren't blocking]
* Grammar -- "It's "its", not "its'""
* Readability -- This triple list comprehension is unnecessary and \
  would be really difficult to debug"


A few other tips for reviewers:
* In larger-scale software development, the code review begins almost before \
  any code is written. These conversations cover architecture, API design \
  and specifications, \
  and provide a blueprint for the work to be done. It's hard to recieve \
  code reviews _after you've written an entire new module_ saying "actually, \
  these two classes should be three classes", \
  because it's requesting a large-scale change that would add _days_ if not \
  _weeks_ to the development time. We are here to experiment with cool \
  science, so be sure to give the code author an "out" if you're asking for \
  large, not-completely-essential changes, by recommending that the refactor become \
  an Issue that can be discussed over time and addressed as the code \
  becomes more mature.
* Give concrete suggestions whenever possible. It's better to say \
  "Consider renaming this to `molecule_to_string`" than "this function \
  name is confusing". 
* If you give open-ended feedback, be active in \
  the discussion thread as the author asks for clarification or proposes solutons.
* If there are three or less instances of the same concern, it's OK to  \
  point out each with a separate comment. However, if there are lots of  \
  repeats, just say "this issue appears throughout the proposed changes". 
* Say some nice things. I've learned a lot from doing code reviews, \
  and it's cool when people point out a trick from my code that \
  they'll use later, or something that they thought was particularly \
  well-designed. As a dispersed team, we don't have the lab banter that \
  usually clears up interpersonal tension, so be positive whenever you \
  have the chance.
* Every codebase has a different history and level of maturity, and \
  each PR should ensure that the changes improve its overall quality a \
  little bit. So adjust the level of the review based on what stage \
  you see the software in. 
* Aim for around ~10 to 15 comments. More than that is overwhelming. 
* Watch out for toolkit- or file-dependent behavior in tests. Just because \
  the example molecule puts all the heavy atoms before the hydrogens doesn't \
  mean that will always be the case.
* You are certainly not limited by the above. If you'd like to learn \
  more, consider reading \
  [Google's Code Review Guide](https://google.github.io/eng-practices/review/reviewer/) \
  and feel free to suggest changes to this message.

### What am I?

I am the Open Force Field Dangerbot. You can find my code and installation instructions
at https://github.com/openforcefield/dangerbot.

MARKDOWN


pp github

client = github.api

repo = github.pr_json[":head"][":repo"][":full_name"] # [":repo"][":full_name"]
number = github.pr_json[":number"] # [":number"]

pr = client.pull_request(repo, number)
comments = client.issue_comments(repo, number)


dangerbot_assigned = false

# Dangerbot defaults to always post to the same message (and just apply edits).
# So, if it's already run, skipping posting would simply delete the previous message.
# Since dangerbot is now activated by requesting review, it's a one-time event, and
# we don't need to worry about previous actions

#dangerbot_already_posted = false
#for comment in comments
#  if comment[":user"][":login"] == "openff-dangerbot"
#    dangerbot_already_posted = true
#  end
#end

#pp pr["requested_reviewers"]
#for reviewer in pr["requested_reviewers"]
#  pp reviewer[":login"]
#  if reviewer[":login"] == "openff-dangerbot"
#    dangerbot_assigned = true
#  end
#end
pp pr["assignees"]
for assignee in pr["assignees"]
  pp assignee[":login"]
  if assignee[":login"] == "openff-dangerbot"
    dangerbot_assigned = true
  end
end


puts "dangerbot assigned?  " + dangerbot_assigned.to_s
#puts "dangerbot already posted? " + dangerbot_already_posted.to_s




if dangerbot_assigned
  reviewers = YAML.load(open('https://raw.githubusercontent.com/openforcefield/dangerbot/master/reviewers.yml'))
  reviewer = reviewers.sample
  #reviewer = "j-wags"
  markdown(PART_ONE + "| @" + reviewer + " |")
  markdown(PART_TWO)
  markdown("This reviewer was selected out of a list of Open Force Field volunteers: "+ reviewers.inspect)
  # client.update_issue(repo, number, :assignee => "j-wags") # {assignee: "j-wags"})
  #client.add_assignees(repo, number, ["j-wags"])
  client.add_assignees(repo, number, [reviewer])
end


#client = github.api


#if !(github.pr_title + github.pr_body).include?("#trivial") and !github.pr_title.include?("[WIP]") and (github.pr_json["requested_reviewers"] == nil)

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

