# vim: ft=ruby

if gitlab.mr_body.empty?
  fail("Please provide a merge request description.")

if gitlab.mr_labels.empty?
  fail("Please add labels to this merge request.")

warn("This MR does not have any assignees yet.") unless gitlab.mr_json.assignee

