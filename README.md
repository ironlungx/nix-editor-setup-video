# Nixd video

## Install nixd, and set nixpkgs in nixpath
```nix
{ pkgs, inputs, ... }: 

{
  environment.systemPackages = [
    pkgs.nixd
  ];

  nix.nixPath = [ "nixpkgs=${inputs.nixpkgs}" ];

  # ...
}
```


## lspconfig nixd setup
```lua
require("lspconfig").nixd.setup({
  cmd = { "nixd" },
  settings = {
    nixd = {
      nixpkgs = {
        expr = "import <nixpkgs> { }",
      },
      formatting = {
        command = { "alejandra" }, -- or nixfmt or nixpkgs-fmt
      },
      -- options = {
      --   nixos = {
      --       expr = '(builtins.getFlake "/PATH/TO/FLAKE").nixosConfigurations.CONFIGNAME.options',
      --   },
      --   home_manager = {
      --       expr = '(builtins.getFlake "/PATH/TO/FLAKE").homeConfigurations.CONFIGNAME.options',
      --   },
      -- },
    },
  },
})
```


## vscode nix-ide setup

FILE PATH: /home/user/.config/Code/User/settings.json

```jsonc
{
  "nix.serverPath": "nixd",
  "nix.enableLanguageServer": true,
  "nix.serverSettings": {
    "nixd": {
      "formatting": {
        "command": [ "alejandra" ], // or nixfmt or nixpkgs-fmt
      },
      // "options": {
      //    "nixos": {
      //      "expr": "(builtins.getFlake \"/PATH/TO/FLAKE\").nixosConfigurations.CONFIGNAME.options"
      //    },
      //    "home_manager": {
      //      "expr": "(builtins.getFlake \"/PATH/TO/FLAKE\").homeConfigurations.CONFIGNAME.options"
      //    },
      // },
    }
  }
}
```

## emacs

```emacs-lisp
(use-package lsp-mode
  :ensure t)

(use-package nix-mode
  :ensure t
  :hook
  (nix-mode . lsp-deferred) ;; So that direnv integration with `envrc-mode' will work
  :config
  (setq lsp-nix-nixd-server-path "nixd")) ;; Note: make sure that nil is not in your $PATH

;; For Tree Sitter
(use-package nix-ts-mode
  :ensure t
  :mode "\\.nix\\'"
  :hook
  (nix-ts-mode . lsp-deferred) ;; So that envrc mode will work
  :config
  (setq lsp-nix-nixd-server-path "nixd")) ;; Note: make sure that nil is not in your $PATH
```

https://emacs-lsp.github.io/lsp-mode/page/lsp-nix/#installation
