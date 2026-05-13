# classickong.r65

An R65 port of Classic Kong Complete, a Donkey-Kong-style platformer for the
Super Nintendo. The original was written in C with hand-rolled SNES SDK
(`816-tcc` + WLA-DX); this project re-implements the same game logic in
[R65](https://github.com/BubbleZapGames/R65), a Rust-inspired systems language
for the 65816.

The goal isn't to add features but to faithfully reproduce the original
gameplay frame-for-frame while replacing every line of C with type-safe R65.

## Gameplay

Climb the construction site, dodge barrels and fireballs, grab the hammer to
smash enemies, ride elevators, jump over springs, light rivets and rescue the
princess. Four classic stages plus the Pauline cutscenes and game-over
sequence.

## Building

Requirements:

- [R65 compiler](https://github.com/BubbleZapGames/R65) (`r65c` /
  `python -m r65.compiler.main`)
- [WLA-DX](https://github.com/vhelin/wla-dx) (`wla-65816`, `wlalink`) on
  `$PATH`

Then:

```bash
make            # produce build/CLASSICKONG.R65.smc
make clean      # wipe build/
```

The Makefile runs three stages:

1. `r65c src/main.r65 -o build/main.asm --cfg snes --dbg` — R65 to WLA-DX
   assembly. The `--cfg snes` flag enables hardware-multiply codegen; `--dbg`
   emits source-line debug info.
2. `wla-65816 -o build/main.o build/main.asm` — assemble. Run from `src/` so
   `.INCBIN` paths into `assets/` resolve.
3. `wlalink -S build/main.link build/CLASSICKONG.R65.smc` — link to a 512 KB
   LoROM image.

## Credits

- Original C source — Classic Kong Complete, the C/ASM SNES port that this
  project mirrors. See [classickong](https://github.com/nathancassano/classickong)
- Port to R65 — Bubble Zap Games, 2026.

## License

Apache 2.0. See `LICENSE`.
