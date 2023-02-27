# Per-language configuration settings

I found a neat extension called "Reflow" for Visual Studio code that supports
text auto-fill mode, similar to Emacs `auto-fill-mode`.  It binds Option-Q to
reformat text, just like Emacs.

I wanted to enable automatic text-reflow when I was editing Markdown files, but
I didn't want to enable it for all files.  To do this, I found that you can go
to "Settings" and type "@lang:markdown" and the changes that you make to your
settings at this point (in this case, settings for Reflow) are only applied to
Markdown files.

This translates into a configuration in `settings.json` like this:

```json
  "[markdown]": {
    "rewrap.autoWrap.enabled": false
  },
```
