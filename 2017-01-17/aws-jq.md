# jq and Amazon Web Services

`jq` is a sed-like tool for
matching and manipulating JSON output.

It's really neat, but I find it difficult to learn,
and reading the documentation doesn't seem to help.

The best way to learn it seems to be
via some sort of secret osmosis.

I'm finding it handy for doing ad-hoc processing of
AWS (Amazon Web Services) commands.

For example.
`aws ec2 describe-snapshots --owner-id self`
will give a list of snapshots that you own.
But the output is a big JSON list.

I just wanted to know the date that each snapshot was taken.

```
aws ec2 describe-snapshots --owner-id self |
  jq -r .Snapshots[].StartTime
```
