# 🍿 terminal

Create and toggle terminal windows.

Based on the provided options, some defaults will be set:

- if no `cmd` is provided, the window will be opened in a bottom split
- if `cmd` is provided, the window will be opened in a floating window
- for splits, a `winbar` will be added with the terminal title

![image](https://github.com/user-attachments/assets/afcc9989-57d7-4518-a390-cc7d6f0cec13)

<!-- docgen -->

## ⚙️ Config

```lua
---@class snacks.terminal.Config
---@field win? snacks.win.Config
---@field override? fun(cmd?: string|string[], opts?: snacks.terminal.Opts) Use this to use a different terminal implementation
{
  win = { style = "terminal" },
}
```

## 🎨 Styles

### `terminal`

```lua
{
  bo = {
    filetype = "snacks_terminal",
  },
  wo = {},
  keys = {
    gf = function(self)
      local f = vim.fn.findfile(vim.fn.expand("<cfile>"))
      if f == "" then
        Snacks.notify.warn("No file under cursor")
      else
        self:close()
        vim.cmd("e " .. f)
      end
    end,
    term_normal = {
      "<esc>",
      function(self)
        self.esc_timer = self.esc_timer or (vim.uv or vim.loop).new_timer()
        if self.esc_timer:is_active() then
          self.esc_timer:stop()
          vim.cmd("stopinsert")
        else
          self.esc_timer:start(200, 0, function() end)
          return "<esc>"
        end
      end,
      mode = "t",
      expr = true,
      desc = "Double escape to normal mode",
    },
  },
}
```

## 📚 Types

```lua
---@class snacks.terminal.Opts: snacks.terminal.Config
---@field cwd? string
---@field env? table<string, string>
---@field interactive? boolean
```

## 📦 Module

```lua
---@class snacks.terminal: snacks.win
---@field cmd? string | string[]
---@field opts snacks.terminal.Opts
Snacks.terminal = {}
```

### `Snacks.terminal()`

```lua
---@type fun(cmd?: string|string[], opts?: snacks.terminal.Opts): snacks.terminal
Snacks.terminal()
```

### `Snacks.terminal.open()`

Open a new terminal window.

```lua
---@param cmd? string | string[]
---@param opts? snacks.terminal.Opts
Snacks.terminal.open(cmd, opts)
```

### `Snacks.terminal.toggle()`

Toggle a terminal window.
The terminal id is based on the `cmd`, `cwd`, `env` and `vim.v.count1` options.

```lua
---@param cmd? string | string[]
---@param opts? snacks.terminal.Opts
Snacks.terminal.toggle(cmd, opts)
```
