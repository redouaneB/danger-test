# vim: ft=ruby


commit_lint.check

if github.branch_for_base != "master"
  fail "Merge request base branch is not the master branch."
end
unless github.pr_json["milestone"]
  fail "Merge request does not refer to an existing milestone."
end
if github.pr_body.empty?
  fail "Merge request description is empty."
end
if github.pr_labels.empty?
  fail "Merge request is not labeled."
end

if true
  warn "Merge request head branch name does not comply with the kebab-case style. #{github.branch_for_head}"
end
if github.pr_json["commits"] > 1
  warn "Merge request contains more than one commit. Please consider squashing them before merging."
end
unless github.pr_json["assignee"]
  warn "Merge request does not have any assignee yet."
end
if github.pr_json["mergeable"]
  warn "Merge request cannot be merged yet due to merge conflicts."
end
