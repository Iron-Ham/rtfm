# The Secret to Great Commit Messages
## And why crafting good commit messages is an investment worth making

There's a lot that's been said about commit messages, and in general, it's best to follow the seven rules of great commit messages by Chris Beams:

1. Separate subject from body with a blank line.
2. Limit the subject line to 50 characters.
3. Capitalize the subject line.
4. Do not end the subject line with a period.
5. Use the imperative mood in the subject lin.e
4. Wrap the body at 72 characters.
1. Use the body to explain *what* and *why* instead of *how*

Rules 1-6 can be summarized as "use good formatting". You may have seen Tim Pope's "model" commit message -- learn it and live by it, because it's good. Damn good.

    Capitalized, short (50 chars or less) summary

    More detailed explanatory text, if necessary.  Wrap it to about 72
    characters or so.  In some contexts, the first line is treated as the
    subject of an email and the rest of the text as the body.  The blank
    line separating the summary from the body is critical (unless you omit
    the body entirely); tools like rebase can get confused if you run the
    two together.

    Write your commit message in the imperative: "Fix bug" and not "Fixed bug"
    or "Fixes bug."  This convention matches up with commit messages generated
    by commands like git merge and git revert.

    Further paragraphs come after blank lines.

    - Bullet points are okay, too

    - Typically a hyphen or asterisk is used for the bullet, followed by a
      single space, with blank lines in between, but conventions vary here

    - Use a hanging indent

Rule 7 is special -- it deals with the content of the commit message, not the format.

### The *why* is the secret behind the best commit messages, and that deserves special attention.

Try to be mindful that commit messages are for **two** audiences: code reviewers and future developers. Aim to give these audiences context ot understand *why* you made the decisions that you did. The investment in writing high-quality commit messages pays increasing dividends as the team changes or you forget things. It's worth the time and the effort.

Great commit messages will **explain other approaches** that were considered, where approriate, and discuss the advantages and disadvantages of each approach. Although this helps get your commit through code review it also gives future developers more context to make informed decisions about changes to the code.

If your commit message gets too long, you should break your commit into smaller, [atomic commits](http://en.wikipedia.org/wiki/Atomic_commit).

If you borrow a solution from StackOverflow, include its URL in the commit message.

In summary, if you:

* properly format your commit messages
* explain the what and why of the change
* discuss any difficulties or tradeoffs made
* mention alternative solutions considered
* reference any relevant commits or online resources

your team and future self will thank you, and your investment will pay back dividends in speed, quality, and productivity.
