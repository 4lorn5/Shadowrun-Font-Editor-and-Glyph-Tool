
## Shadowrun Font Editor and Glyph Tool

The Shadowrun Font Editor and Glyph Tool is an editor for Shadowrun on the Sega Genesis. It allows users to export the game's font and import the same file with custom glyphs. It also allows for the creation of five additional glyphs directly in the tool.

## Features:

- Load ROM in common formats (.bin, .gen, and .md)
- Export and import font as .bin files (Tile Molester or YY-CHR recommended for edition)
- New font preview on import
- Up to 6 new glyphs can be drawn directly in the tool
- Define glyph control codes directly in the tool for use in ASCII dialogue in-game
- Built-in ROM expansion option for extra glyph space (risky but it's there)
- Generates ready to play ROM file (requires original ROM) with updated checksum
- HTML (run a lightweight, portable file; or use directly online)

## Running the Editor:

* HMTL: Just save file as .html and double-click from your local folder; it will open in your default browser.
* Online: The editor is live at: https://4lorn5.github.io/Shadowrun-Font-Editor-and-Glyph-Tool/

## Usage:

**- CHOOSE ROM:** Choose your Shadowrun (.bin, .gen, and .md extensions accepted).

**- ORIGINAL FONT:** Displays the game's font family.

**- DUMP FONT:** Exports the game's font in .bin format. Tile editors like Tile Molester and YY-CHR are recommended since they have codecs that display the 4bpp format.

**- IMPORT FONT:** Import the .bin file an with edited font. The imported .bin must not exceed the originally exported .bin file size.

**- CUSTOM GLYPHS:** Whether you import a new .bin font file or not, you can draw up to 6 additional glyphs to be used in game, but note that depending on glyph complexity, only up to 5 may work. This is because the game ROM has very little in the way of free space and the new glyphs are moved to the EOF. If additional space is required for custom glyphs, use the "Allow Rom Expansion" option (risky but it should work).

Custom glyphs can be drawn directly in the tool (flip and rotation options are available as well). Once you're satisfied with a glyph, you can then define the character control code (in the 0x81 to 0x86 range) to use it in the game's ASCII text. To draw, use the mouse on the glyph slot panel on the right; the panels on the left will update in real time with your changes.

**- EXPORT PATCHED ROM:** Generate a ready-to-play "Shadowrun (USA)_patched" ROM (requires the original ROM and changes applied).


## Documentation

Shadowrun stores the compressed font at pointer 0xC0CA with a very small byte budget, and upload tiles to VRAM at a fixed base (0xB560). The game has almost no free space so there's a very limited range for extra glyphs. The tool allows for decompression and compression of its 85-tile font; when creating new glyphs, it relocates them to the EOF and patches the pointer. For the new glyphs, it builds a separate compressed tile blob (not part of the main font), shifts the font's VRAM base to make for new space, and rewrites the character table entries (92+) to point at the extra tiles.

A 68K trampoline replaces the render branch at 0xC11E to expand the valid range and keep the original flow, and a lazy‑init stub uploads the extra blob once via the game's decompressor (JSR $1530) before jumping back. If using a ROM without expansion, the code+blob is packed into the last suitable 0xFF run; if using an expanded ROM, it appends safely and updates the ROM header and checksum.

## Special thanks

- Pretty much any Motorola 68K reference I could find, but shout out to And-0 for his Mega Drive programming git:
https://github.com/And-0/awesome-megadrive
