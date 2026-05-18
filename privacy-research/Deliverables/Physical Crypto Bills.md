*This is a project to create "bills" which store crypto and can be physically given to someone. Ideally these should be setup so that someone can access this crypto with no prior knowlege*

### Bill creation
*This must be as secure as possible. Preference for airgapped device and some method of ourside world entropy*

Current plan:
- use raspberry pi zero to control a printer which prints off a seed phrase, that can then be stored in a sealed packet to create the bill
- The bill itself will be a seed phrase in a sealed, tamper evident packet. There should be a layer of aluminum foil or some sturdy reflective material to prevent seed phrases from being read by using a strong light shone through the paper. The packets internally should also have some waterproofing



### Instructions
#TODO create idiot-proof instructions for someone who receives a crypto bill

- link to an instruction website?
- warning that exposing seed phrase means access to funds is exposed
- explanation about how to verify bill has value before accepting it - perhaps QR code
	- this just uses the public key or view key
- Suggestions for wallets
- warning about heat, cold, or wetness could all damage the bill. Esp heat if thermal printer
	- if thermal printer is used, determine temperature at which printing occurs (also cold, if relevant)

### Testing
- Environmental testing - what are the limits of these bills
- Are seed phrases generated correctly from randomness? 
- Are public keys generated correctly from seed phrases?

## Status
*Last updated: may 17 2026*
Current status is that a raspberry pi zero v1.3 exists, but will not boot. It seems to be an issue with the SD card.

### Action items
 1. Get raspberry pi zero working and loading an OS
 2. Locate or buy a splitter for USB peripherals for the raspberry pi
	- depends on (1), otherwise we will may need a different board and no need to waste money on that
3. Generate a seed phrase from "random" inputs
4. Create system for generating "randomness"
	1. Consider purchasing d20's and daily pill-holders (for rolling lots of them quickly)
		1. How many d20's are needed? #TODO 