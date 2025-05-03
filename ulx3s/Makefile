VERILOG = \
	pll.v \
	hdmi.v \
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

PIN_DEF = ulx3s.lpf

DEVICE ?= 85k

BUILDDIR = bin

DEFINES = -DULX3S -DDIGILENT_PS2_PMOD

compile: $(BUILDDIR)/toplevel.bit

prog: $(BUILDDIR)/toplevel.bit
	fujprog $^

$(BUILDDIR)/toplevel.json: $(VERILOG)
	mkdir -p $(BUILDDIR)
	yosys -p "verilog_defines $(DEFINES); read_verilog $(VERILOG); synth_ecp5 -top msx -json $@" $^

$(BUILDDIR)/toplevel.config: $(PIN_DEF) $(BUILDDIR)/toplevel.json
	nextpnr-ecp5 --${DEVICE} --package CABGA381 --timing-allow-fail --freq 25 --textcfg  $@ --json $(filter-out $<,$^) --lpf $<

$(BUILDDIR)/toplevel.bit: $(BUILDDIR)/toplevel.config
	ecppack --compress $^ $@

clean:
	rm -rf ${BUILDDIR}

.SECONDARY:
.PHONY: compile clean prog


