## Check for Tex Itself

``` which latexmk ``` should give you `/Library/Tex/texbin/latexmk`

make sure this is in `$PATH`

also check for `/Applications/TeX` which means you have `MacTex` installed

## Upgrading

` sudo tlmgr update --self`
` sudo tlmgr update --all`

## Installing

[see the MacTex page](https://www.tug.org/mactex/)

generally install the entire huge package, see on the above page for further options

## add zathura doc viewer for LaTeX
[see this reddit link](https://www.reddit.com/r/vim/comments/7c7wd9/vim_vimtex_zathura_on_macos/) for more on the below commands 
``` 
brew tap zegervdv/zathura
brew install zathura --with-synctex
brew install zathura-pdf-poppler
```
also symlink the libs

```
mkdir -p (brew --prefix zathura)/lib/zathura
ln -s (brew --prefix zathura-pdf-poppler)/libpdf-poppler.dylib (brew --prefix zathura)/lib/zathura/libpdf-poppler.dylib
```

## add nvim plugin

`use 'lervag/vimtex'`

## add to nvim-treesitter

``` lua
-- Treesitter Plugin Setup
require('nvim-treesitter.configs').setup {
    ensure_installed = { "bash", "c", "cmake", "css", "dockerfile", "go", "gomod", "gowork", "hcl", "help", "html",
         "javascript", "json", "lua", "latex", "make", "markdown", "python", "regex", "ruby", "rust", "toml", "vim", "yaml",
         "proto" },
    auto_install = true,
    highlight = {
        enable = true,
    },
    incremental_selection = {
        enable = true,
        keymaps = {
            init_selection = "<S-Tab>", -- normal mode
            node_incremental = "<Tab>", -- visual mode
            node_decremental = "<S-Tab", -- visual mode
        },
    },
    ident = { enable = true },
    rainbow = {
        enable = true,
    }
}

errm actually dont do this as we dont want tree sitter to syntax tex vimtex does this....
```
## useful tutorial
[part 4](https://www.ejmastnak.com/tutorials/vim-latex/vimtex/) of a 7 parter about nvim and Tex
 
## add to `init.vim`
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQwNDg5ODg1N119
-->