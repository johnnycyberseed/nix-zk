- A process that runs as `root` providing a safe shared nix installation.
- The Daemon...
	- enabling multi-user support by being the only process that can write to [[Nix Store]]
	- is given requests by the user via the [[nix CLI]]
	- downloads pre-built binaries from caches (e.g. https://cache.nixos.org)
	- ensures only trusted [[substituters]] (as configured in [[nix.conf]]) are used.
	- ensures only [[trusted users]] are allowed to perform privileged operations.
	- performs compilation and evaluation of packages
	- parallelizes builds
- Is configured via [[nix Configuration]] Configuration
	- /etc/nix/[[nix.conf]] — managed by the Determinate Nix installer; don't touch,
	- `/etc/nix/nix.custom.conf` — place to put your customizations.
-
	-
- {{renderer :mermaid_68575bb0-cfea-4782-bd5f-69bc119bba8e, 6}}
  collapsed:: true
	- ```mermaid
	  graph LR
	    subgraph User Space
	      User["👤 User (e.g. johnnycyberseed)"]
	      CLI["nix CLI (e.g. nix build/run/profile)"]
	      CLI -->|request| Daemon
	    end
	  
	    subgraph System Space
	      Daemon["nix-daemon Runs as root or nixbld"]
	      Daemon -->|evaluates| Expr["Nix Expression / Flake"]
	      Expr --> Derivation["Build Derivation"]
	      Derivation --> Store["/nix/store (immutable store)"]
	      Derivation --> Sandbox["Build Sandbox (isolated env)"]
	      Daemon -->|fetch| I["Substituters (e.g. cache.nixos.org, Cachix)"]
	    end
	  
	    subgraph Config Files
	      NixConf["/etc/nix/nix.conf"]
	      NixCustomConf["/etc/nix/nix.custom.conf"]
	      NixConf --> Daemon
	      NixCustomConf --> Daemon
	    end
	  
	    User -->|runs| CLI
	    Store -->|symlinks| Profile["~/.nix-profile"]
	  ```
	-