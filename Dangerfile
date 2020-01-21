require 'open-uri'
require 'yaml'



# The following is taken from GitLab's reviewer roulette:
# https://gitlab.com/gitlab-org/gitlab-foss/blob/master/danger/roulette/Dangerfile
# There is a MUCH more sophisticated system there, which we might begin
# adopting more of if the need arises.

MESSAGE = <<MARKDOWN
## Reviewer roulette

Changes that require review have been detected! A merge request is normally
reviewed by both a reviewer and a maintainer in its primary category (e.g.
~frontend or ~backend), and by a maintainer in all other categories.
MARKDOWN

CATEGORY_TABLE_HEADER = <<MARKDOWN

To spread load more evenly across eligible reviewers, Danger has randomly picked
a candidate for each review slot. Feel free to override this selection if you
think someone else would be better-suited, or the chosen person is unavailable.

Once you've decided who will review this merge request, mention them as you
normally would! Danger does not (yet?) automatically notify them for you.

| Reviewer |
|----------|
MARKDOWN

unless (github.pr_title + github.pr_body).include?("#trivial")?
  reviewers = YAML.load(open('https://raw.githubusercontent.com/openforcefield/dangerbot/master/reviewers.yml'))
  # reviewer = reviewers.sample
  message(reviewers.inspect)

  markdown(MESSAGE)
  markdown(CATEGORY_TABLE_HEADER)
  markdown("|"+reviewers.sample+"|") 
  markdown("This reviewer was selected out of a list of volunteer Open Force Field developers: "+ reviewers.inspect)
end






# message(reviewers.sample)
# puts(github.pr_json["assignee"])

