# nix-firefox-video

```nix
# home.nix 
                                                                                           
{ pkgs, inputs, ... }:

{
  programs.firefox = {
    enable = true;
    profiles.yurii = {

      search.engines = {
        "Nix Packages" = {
          urls = [{
            template = "https://search.nixos.org/packages";
            params = [
              { name = "type"; value = "packages"; }
              { name = "query"; value = "{searchTerms}"; }
            ];
          }];

          icon = "${pkgs.nixos-icons}/share/icons/hicolor/scalable/apps/nix-snowflake.svg";
          definedAliases = [ "@np" ];
        };
      };
      search.force = true;

      bookmarks = [
        {
          name = "wikipedia";
          tags = [ "wiki" ];
          keyword = "wiki";
          url = "https://en.wikipedia.org/wiki/Special:Search?search=%s&go=Go";
        }
      ];

      settings = {
        "dom.security.https_only_mode" = true;
        "browser.download.panel.shown" = true;
        "identity.fxaccounts.enabled" = false;
        "signon.rememberSignons" = false;
      };

      userChrome = ''                         
        /* some css */                        
      '';                                      

      extensions = with inputs.firefox-addons.packages."x86_64-linux"; [
        bitwarden
        block-origin
        ponsorblock
        arkreader
        ridactyl
        outube-shorts-block
      ];

    };
  };
}

```


```nix
# home-manager flake, generated by `home-manager init`

{
  description = "Home Manager configuration of vimjoyer";

  inputs = {
    nixpkgs.url = "github:nixos/nixpkgs/nixos-unstable";
    home-manager = {
      url = "github:nix-community/home-manager";
      inputs.nixpkgs.follows = "nixpkgs";
    };

    firefox-addons = {
      url = "gitlab:rycee/nur-expressions?dir=pkgs/firefox-addons";
      inputs.nixpkgs.follows = "nixpkgs";
    };
  };

  outputs = { nixpkgs, home-manager, ... }@inputs:
    let
      system = "x86_64-linux";
      pkgs = nixpkgs.legacyPackages.${system};
    in {
      homeConfigurations."vimjoyer" = home-manager.lib.homeManagerConfiguration {
        inherit pkgs;

        extraSpecialArgs = { inherit inputs; };

        modules = [ ./home.nix ];

      };
    };
}
```

```bash
nix flake show "gitlab:rycee/nur-expressions?dir=pkgs/firefox-addons"
```
