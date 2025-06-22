- A process that runs as `root` providing a safe shared nix installation.
- The Daemon...
	- is the only process that can write to [[Nix Store]]
	- is given requests by the user via the [[nix CLI]]
	-
- Configuration
	- `/etc/nix/nix.conf` — managed by the Determinate Nix installer; don't touch,
	- `/etc/nix/nix.custom.conf` — place to put your customizations.
	-
	-
- {{renderer :mermaid_68575bb0-cfea-4782-bd5f-69bc119bba8e, 6}}
	- ```mermaid
	  graph LR
	    subgraph User Space
	      A["👤 User (e.g. johnnycyberseed)"]
	      B["nix CLI (e.g. nix build/run/profile)"]
	      B -->|request| D
	    end
	  
	    subgraph System Space
	      D["nix-daemon Runs as root or nixbld"]
	      D -->|evaluates| E["Nix Expression / Flake"]
	      E --> F["Build Derivation"]
	      F --> G["/nix/store (immutable store)"]
	      F --> H["Build Sandbox (isolated env)"]
	      D -->|fetch| I["Substituters (e.g. cache.nixos.org, Cachix)"]
	    end
	  
	    subgraph Config Files
	      J["/etc/nix/nix.conf"]
	      K["/etc/nix/nix.custom.conf"]
	      J --> D
	      K --> D
	    end
	  
	    A -->|runs| B
	    G -->|symlinks| L["~/.nix-profile"]
	  ```
	-