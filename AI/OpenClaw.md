
- Upon installation, openclaw recommends running these regulary:
	- `openclaw security audit --deep`
	- `openclaw security audit --fix`
- Other neat things from install
	- "openclaw is not a hostile multi-tenant boundary by default"
	- "keep secrets out of the agent's reachable filesystem"
		- [ ] is it possible for an agent to elevate its own privileges?

## Models

### Remote connection
- if you have an API key or something you can point to

### Ollama
- installing this to run it locally
- appears that a GB of Ram is enough for about 2B parameters - its recommended for laptop with ~16GB Ram to do 7B size model
- Looks like this might kinda suck
