# polychrome.nvim

A colorscheme creation micro-framework.

## Usage

```lua
-- colors/mytheme.lua

local Colorscheme = require('polychrome').Colorscheme

-- Quite a few different color systems are supported:
--
--   RGB hex string (vim natively supports; just pass as a normal string)
--   Raw RGB values (use the `rgb` helper)
--   Linear (non-gamma-corrected) RGB (`lrgb`)
--   HSL (`hsl`)
--   Oklab (`oklab`)
--   Oklch (`oklch`)
--
-- You can mix and match -- your usage as you please. The helpers are
-- automatically injected into the colorscheme definition context. However, if
-- you prefer to import the helpers explicitly, you can do:
--
--   local rgb = require('polychrome').rgb

Colorscheme:define('mytheme', function ()
    -- most groups can be specified like this
    Constant { fg = rgb(0, 255, 0), bg = '#ff0000' }

    -- add a group for a GUI formatting feature
    Underline { gui = 'underline' }

    -- link a group to another one (equivalent to `:hi link Boolean Constant`)
    Boolean { Constant }

    -- groups that use `@` or other special characters (ex. treesitter groups)
    -- need a slightly different syntax: _'<name>' instead of <name>
    _'@punctuation.delimiter' { fg = rgb(0, 255, 0), bg = oklch(0, 0, 0) }

    -- linking groups with special characters works similarly to normal
    _'@punctuation.delimiter' { Constant }

end):apply()
```

### Live Preview

You can turn on a live preview of your colorscheme via the `:StartEditing`
command, or `require('polychrome').StartPreview()` from a Lua context. Running
this command does the following:

  1. Runs the contents of the current buffer, and reapplies any colorscheme
     defined anywhere within it (even via `require`).
  2. Get the names of all active highlight groups, and applies those groups to
     all literal strings in the current buffer matching those names (ie. all
     occurences of `Comment` in the buffer will be highlighted with the
     "Comment" highlight group)

`StartEditing()` registers these actions via autocmd (specifically
`TextChanged`, `TextChangedI`, `TextChangedP`, and `TextChangedT`), and
throttles reloads to every 500ms by default, shared across the autocommands.

You can deactivate the live editing mode with `:StopEditing`, or
`require('polychrome').StopPreview()` from a Lua context.

## Finding highlight groups

There isn't a single authoritative source for all the most common highlight
groups used in Vim/Neovim, so here are some good places to get at least some of
the most common groups:

```
# docs for the most basic recommended highlight groups
:h group-name
:h highlight-groups

# docs for treesitter highlight groups
# also check https://github.com/nvim-treesitter/nvim-treesitter/blob/master/CONTRIBUTING.md#highlights
:h treesitter-highlight-groups

# show all highlight groups in your current (neo)vim session
:so $VIMRUNTIME/syntax/hitest.vim
```
