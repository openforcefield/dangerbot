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
text, or if a reviewer is already assigned.

Once you've decided who will review this merge request, mention them as you
normally would! Danger does not (yet?) automatically notify them for you.

| Reviewer |
|----------|
MARKDOWN

if !(github.pr_title + github.pr_body).include?("#trivial") and !github.pr_title.include?("[WIP]") and (github.pr_json["assignee"] == nil) 
  reviewers = YAML.load(open('https://raw.githubusercontent.com/openforcefield/dangerbot/master/reviewers.yml'))
  markdown(MESSAGE)
  markdown(CATEGORY_TABLE_HEADER + "| @j-wags |\n| " + reviewers.sample + " |")
  markdown("This reviewer was selected out of a list of Open Force Field volunteers: "+ reviewers.inspect)
end



  # reviewer = reviewers.sample
  # message(reviewers.inspect)

# message(reviewers.sample)
# puts(github.pr_json["assignee"])

