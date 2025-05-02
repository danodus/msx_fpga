VERILOG = \
	pll.v \
	../src/msx.v \
	../src/gamerom.v \
	../src/keyboard.v \
	../src/keymap.v \
	../src/keyram.v \
	../src/keyrom.v \
	../src/ps2.v \
	../src/ram.v \
	../src/rom.v \
	../src/video.v \
	../src/vram.v \
	../src/Z80/tv80_core.v \
	../src/Z80/tv80n.v \
	../src/Z80/tv80_mcode.v \
	../src/Z80/tv80_reg.v \
	../src/Z80/tv80_alu.v

PIN_DEF = gma1evb.ccf

BUILDDIR = bin

compile: $(BUILDDIR)/toplevel.bit

prog: $(BUILDDIR)/toplevel.bit
	openFPGALoader -b olimex_gatemateevb $^

$(BUILDDIR)/toplevel.json: $(VERILOG)
	mkdir -p $(BUILDDIR)
	yosys -p "synth_gatemate -top msx -json $@ -nomx8 -nomult -luttree" $^

$(BUILDDIR)/toplevel.config: $(PIN_DEF) $(BUILDDIR)/toplevel.json
	nextpnr-himbaechel --device CCGM1A1 --timing-allow-fail --randomize-seed --freq 10 --router router2 --vopt out=$@ --json $(filter-out $<,$^) --vopt ccf=$<

$(BUILDDIR)/toplevel.bit: $(BUILDDIR)/toplevel.config
	gmpack $^ $@

clean:
	rm -rf ${BUILDDIR}

.SECONDARY:
.PHONY: compile clean prog


