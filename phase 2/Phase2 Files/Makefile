vhd_to_fsm:
	rename .vhd .fsm *.vhd

sdeta.vbe: sdet.fsm
	@echo "Encoding -a -> $@"
	syf -CEV -a sdet
sdeto.vbe: sdet.fsm
	@echo "Encoding -o -> $@"
	syf -CEV -o sdet
sdetj.vbe: sdet.fsm
	@echo "Encoding -j -> $@"
	syf -CEV -j sdet
sdetr.vbe: sdet.fsm
	@echo "Encoding -r -> $@"
	syf -CEV -r sdet
sdetm.vbe: sdet.fsm
	@echo "Encoding -m -> $@"
	syf -CEV -m sdet

all: sdeta.vbe\
	sdeto.vbe\
	sdetr.vbe\
	sdetj.vbe\
	sdetm.vbe

sdet_boom: sdeta_b.vbe\
		sdeto_b.vbe\
		sdetr_b.vbe\
		sdetj_b.vbe\
		sdetm_b.vbe


%_b.vbe: %.vbe	
	@echo "Boolean Optimization -> $@"
	boom -V -d 50 $* $*_b > $*_boom.out

sdet_boog: sdeta_b.vst\
		sdeto_b.vst\
		sdetr_b.vst\
		sdetj_b.vst\
		sdetm_b.vst


%.vst:	%.vbe paramfile.lax
	@echo " Logical Synthesis -> $@"
	boog -x 1 -l paramfile $* > $*_boog.out

sdet_loon: sdeta_b_l.vst\
		sdeto_b_l.vst\
		sdetr_b_l.vst\
		sdetj_b_l.vst\
		sdetm_b_l.vst

%_l.vst: %.vst paramfile.lax
	@echo " Netlist Optimization -> $@"
	loon -x 1 $* $*_l paramfile > $*_loon.out 

sdet_proof : sdeta_b_net.vbe sdetj_b_net.vbe sdetm_b_net.vbe \
			sdeto_b_net.vbe sdetr_b_net.vbe
%_b_net.vbe : %_b.vst %.vbe
	@echo " Formal checking -> $@ "
	flatbeh $*_b $*_b_net > $*_flatbeh.out
	proof -d $* $*_b_net > $*_proof.out

sdet_scapin : sdeta_scan.vst sdetj_scan.vst sdetm_scan.vst \
			sdeto_scan.vst sdetr_scan.vst
	@echo "Scan-path insertion <-- Inserted"

%_scan.vst : %.vst scan.path
	@echo "    scan-path insertion -> $@ "
	scapin -VRB $* scan $*_scan > scapin.out


clean:
	rm -f *.vbe *.enc *~
