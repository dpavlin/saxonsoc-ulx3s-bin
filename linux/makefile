VERILOG = Ulx3sLinux.v pll_linux.v

IDCODE ?= 0x21111043 # 12f

compile : bin/toplevel.bit

prog: bin/toplevel.bit
	ujprog $<

bin/toplevel.json: ${VERILOG}
	mkdir -p bin
	yosys \
		-p "synth_ecp5 -json $@" \
		${VERILOG}

bin/toplevel.config: bin/toplevel.json
	nextpnr-ecp5 \
		--json $< \
		--textcfg $@ \
		--lpf ulx3s_v20.lpf \
		--25k \
		--freq 50 \
		--package CABGA381 \
		--lpf-allow-unconstrained

bin/toplevel.bit: bin/toplevel.config
	ecppack --idcode $(IDCODE) $< $@

pll:
	ecppll \
                -i 25 \
                -o 40 \
		--clkout1 40 \
		--phase1 45 \
                -n  pll_linux\
                -f pll_linux.v
clean:
	$(RM) -rf bin
