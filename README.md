maildropqueue
=============

SubEthaSMTP-based Mail Drop that executes actions on email reception

In essense, it's a java-based daemon implemented mostly on the back of SubEthaSMTP, but
offers some configuration for its actions.  It's relatively new.

The general idea is that you have a list of actions:
1) ftp a file somplace (ie publicize something that was emailed)
2) log that something was received (converting a mail message to a log entry)
3) execute a script because we cannot predict what you'll need

These actions are then spelled out as names:

<ACTION ID="uniqueID" name="easier name"> action-specific stuff </ACTION>

A bunch of these make up a set of actions.

Next, you have reasons to trigger these:
1) sender name
2) recipient name
3) filename of attachment

The logic is designed to allow compound statements (this AND that) but not yet configurable.
The reason is that the triggers are mostly "or statements", and without a really complex
"and" situation, I haven't found the need to go that far.

Each trigger activates one or more actions:

<trigger><condition .../><action>uniqueID</action><action>unique2</action></trigger>

Building It
===========

If you've gotten this far, you've got the source code.  The only thing remaining is to build it:

1) autoreconf -vfi
2) make
3) make check   (optional)


