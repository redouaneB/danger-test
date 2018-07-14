# vim: ft=ruby

# TODO: Handle gitlab CI.

require 'git_diff_parser'

max_modified_lines = 200
max_modified_files = 5
kebab_case = /^[a-z]+(-[a-z]+)*$/
rally_id =  /(US|DE)[0-9]{6}/
extra_space =  /[^\+\s]\s{2,}/

commit_lint.check
todoist.warn_for_todos
todoist.print_todos_table

if github.branch_for_base != "master"
  fail "Merge request base branch is not the master branch."
end
unless github.pr_json.milestone
  fail "Merge request does not refer to an existing milestone."
end
if github.pr_body.empty?
  fail "Merge request description is empty."
end
if github.pr_labels.empty?
  fail "Merge request is not labeled."
end

git.commits.each do |commit|
  if commit.message !~ rally_id
    warn "Commit #{commit.sha} does not mention a user story or a defect identifier."
  end
end

if github.branch_for_head !~ kebab_case
  warn "Merge request head branch name does not comply with the kebab-case style."
end
if github.pr_json.commits > 1
  warn "Merge request contains more than one commit. Please consider squashing them before merging."
end
unless github.pr_json.assignee
  warn "Merge request does not have any assignee yet."
end
unless github.pr_json.mergeable
  warn "Merge request cannot be merged yet due to merge conflicts."
end
if github.pr_title !~ rally_id
  warn "Merge request title does not mention a user story or a defect identifier."
end
if git.lines_of_code > max_modified_lines
  warn "Merge request scale is too big. Please consider splitting it."
end

patches = GitDiffParser::Patches.parse(github.pr_diff)
patches.each do |patch|
  next if patch.file != "Dangerfile"
  patch.changed_lines.each do |line|
    warn("Extraneous space.", file: patch.file, line: line.number) if line.content =~ extra_space
  end
end
