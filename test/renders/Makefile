# RCSid $Id: Makefile,v 1.20 2019/02/13 17:24:06 greg Exp $
#
# Render and primitive type testing
#

# Number of processes to use on tests that run multi-core
NPROC = 2

# Image reduction for comparisons
RDU_PFILT = pfilt -1 -r 1 -x 128 -y 128 -pa 1

# Image comparison command
IMG_CMP = radcompare -rms 0.07 -max 1.5

# Default target is to test everything
all:	test-xform test-oconv test-rad test-rfluxmtx test-rpiece \
test-rpict test-mkpmap \
test-mixtex-def test-mixtex-fish test-mixtex-plan test-mixtex-rplan \
test-trans2-def test-trans2-fish test-dielectric-def \
test-dielectric-fish test-glass-def test-glass-fish test-glass-up \
test-tfunc-def test-tfunc-fish test-inst-def test-inst-fish \
test-mesh-def test-mesh-cyl test-mirror-fish test-mist-def \
test-trans-def test-trans-fish test-patterns-def test-patterns-plan \
test-rtrace

clean:
	rm -f *.oct *.amb *_ill.dat blinds_ill?.dat *_*.hdr *.unf \
*.[cg]pm{,.leaf} inst_rad.txt combined.rad rfmirror.mtx

### Test Aliases ###

test-rpict:	test-mirror-fish

test-rpiece test-mkpmap:	test-mixtex-def

### Special test of xform ###

test-xform:	combined.rad
	radcompare -max 0.04 ref/combined.rad combined.rad

combined.rad:	combined_scene.rad
	xform -f combined_scene.rad | grep -v '^[ 	]*#' > combined.rad

### Special test of rad ###

test-rad:	inst.rif
	rad -n -s -e inst.rif > inst_rad.txt
	radcompare ref/inst_rad.txt inst_rad.txt

### Special test of oconv ###

test-oconv:	inst.oct
	radcompare ref/inst.oct inst.oct

### Special test of rtrace ###

test-rtrace:	ref/mirror_fish.hdr  rtmirror_fish.hdr
	$(RDU_PFILT) rtmirror_fish.hdr | $(IMG_CMP) -h ref/mirror_fish.hdr -

rtmirror_fish.hdr:	mirror.oct
	rad -v 0 mirror.rif OPT=mirror.opt 
	vwrays -ff -vf fish.vf -x 2048 -y 2048 | rtrace -n $(NPROC) @mirror.opt \
-ffc -x 2048 -y 2048 mirror.oct | pfilt -1 -e +3 -r .6 -x /2 -y /2 \
> rtmirror_fish.hdr
	rm -f mirror.opt

### Special test for rfluxmtx (and rcontrib) ###

test-rfluxmtx:	ref/rfmirror.mtx rfmirror.mtx
	radcompare -max .4 -rms .05 ref/rfmirror.mtx rfmirror.mtx

rfmirror.mtx:	mirror.rad dummysky.rad basic.mat diorama_walls.rad \
closed_end.rad front_cap.rad glass_pane.rad antimatter_portal.rad
	rfluxmtx -n $(NPROC) -ab 2 -lw 1e-4 mirror.rad dummysky.rad \
basic.mat diorama_walls.rad closed_end.rad front_cap.rad \
glass_pane.rad antimatter_portal.rad > rfmirror.mtx

### Octree for dielectric ###

dielectric.oct:	dielectric.rif
	rad -v 0 dielectric.rif

### Reference and test for dielectric view   def ###

test-dielectric-def:	ref/dielectric_def.hdr dielectric_def.hdr
	$(RDU_PFILT) dielectric_def.hdr | $(IMG_CMP) ref/dielectric_def.hdr -

ref/dielectric_def.hdr:
	make dielectric_def.hdr
	$(RDU_PFILT) dielectric_def.hdr > ref/dielectric_def.hdr

dielectric_def.hdr:	dielectric.oct
	rad -v   def dielectric.rif

### End dielectric-def   tests

### Reference and test for dielectric view  fish ###

test-dielectric-fish:	ref/dielectric_fish.hdr dielectric_fish.hdr
	$(RDU_PFILT) dielectric_fish.hdr | $(IMG_CMP) ref/dielectric_fish.hdr -

ref/dielectric_fish.hdr:
	make dielectric_fish.hdr
	$(RDU_PFILT) dielectric_fish.hdr > ref/dielectric_fish.hdr

dielectric_fish.hdr:	dielectric.oct
	rad -v  fish dielectric.rif

### End dielectric-fish  tests

### Octree for   glass ###

glass.oct:	glass.rif
	rad -v 0   glass.rif

### Reference and test for   glass view   def ###

test-glass-def:	ref/glass_def.hdr   glass_def.hdr
	$(RDU_PFILT)   glass_def.hdr | $(IMG_CMP) ref/glass_def.hdr -

ref/glass_def.hdr:
	make   glass_def.hdr
	$(RDU_PFILT)   glass_def.hdr > ref/glass_def.hdr

glass_def.hdr:	glass.oct
	rad -v   def   glass.rif

### End   glass-def   tests

### Reference and test for   glass view  fish ###

test-glass-fish:	ref/glass_fish.hdr   glass_fish.hdr
	$(RDU_PFILT)   glass_fish.hdr | $(IMG_CMP) ref/glass_fish.hdr -

ref/glass_fish.hdr:
	make   glass_fish.hdr
	$(RDU_PFILT)   glass_fish.hdr > ref/glass_fish.hdr

glass_fish.hdr:	glass.oct
	rad -v  fish   glass.rif

### End   glass-fish  tests

### Reference and test for   glass view    up ###

test-glass-up:	ref/glass_up.hdr   glass_up.hdr
	$(RDU_PFILT)   glass_up.hdr | $(IMG_CMP) ref/glass_up.hdr -

ref/glass_up.hdr:
	make   glass_up.hdr
	$(RDU_PFILT)   glass_up.hdr > ref/glass_up.hdr

glass_up.hdr:	glass.oct
	rad -v    up   glass.rif

### End   glass-up    tests

### Octree for    inst ###

inst.oct:	inst.rif
	rad -v 0    inst.rif

### Reference and test for    inst view   def ###

test-inst-def:	ref/inst_def.hdr    inst_def.hdr
	$(RDU_PFILT)    inst_def.hdr | $(IMG_CMP) ref/inst_def.hdr -

ref/inst_def.hdr:
	make    inst_def.hdr
	$(RDU_PFILT)    inst_def.hdr > ref/inst_def.hdr

inst_def.hdr:	inst.oct
	rad -v   def    inst.rif

### End    inst-def   tests

### Reference and test for    inst view  fish ###

test-inst-fish:	ref/inst_fish.hdr    inst_fish.hdr
	$(RDU_PFILT)    inst_fish.hdr | $(IMG_CMP) ref/inst_fish.hdr -

ref/inst_fish.hdr:
	make    inst_fish.hdr
	$(RDU_PFILT)    inst_fish.hdr > ref/inst_fish.hdr

inst_fish.hdr:	inst.oct
	rad -v  fish    inst.rif

### End    inst-fish  tests

### Octree for    mesh ###

mesh.oct:	mesh.rif
	rad -v 0    mesh.rif

### Reference and test for    mesh view   def ###

test-mesh-def:	ref/mesh_def.hdr    mesh_def.hdr
	$(RDU_PFILT)    mesh_def.hdr | $(IMG_CMP) ref/mesh_def.hdr -

ref/mesh_def.hdr:
	make    mesh_def.hdr
	$(RDU_PFILT)    mesh_def.hdr > ref/mesh_def.hdr

mesh_def.hdr:	mesh.oct
	rad -v   def    mesh.rif

### End    mesh-def   tests

### Reference and test for    mesh view   cyl ###

test-mesh-cyl:	ref/mesh_cyl.hdr    mesh_cyl.hdr
	$(RDU_PFILT)    mesh_cyl.hdr | $(IMG_CMP) ref/mesh_cyl.hdr -

ref/mesh_cyl.hdr:
	make    mesh_cyl.hdr
	$(RDU_PFILT)    mesh_cyl.hdr > ref/mesh_cyl.hdr

mesh_cyl.hdr:	mesh.oct
	rad -v   cyl    mesh.rif

### End    mesh-cyl   tests

### Reference and test for    mesh view  cyl2 ###

test-mesh-cyl2:	ref/mesh_cyl2.hdr    mesh_cyl2.hdr
	$(RDU_PFILT)    mesh_cyl2.hdr | $(IMG_CMP) ref/mesh_cyl2.hdr -

ref/mesh_cyl2.hdr:
	make    mesh_cyl2.hdr
	$(RDU_PFILT)    mesh_cyl2.hdr > ref/mesh_cyl2.hdr

mesh_cyl2.hdr:	mesh.oct
	rad -v  cyl2    mesh.rif

### End    mesh-cyl2  tests

### Octree for  mirror ###

mirror.oct:	mirror.rif
	rad -v 0  mirror.rif

### Reference and test for  mirror view  fish ###

test-mirror-fish:	ref/mirror_fish.hdr  mirror_fish.hdr
	$(RDU_PFILT)  mirror_fish.hdr | $(IMG_CMP) ref/mirror_fish.hdr -

ref/mirror_fish.hdr:
	make  mirror_fish.hdr
	$(RDU_PFILT)  mirror_fish.hdr > ref/mirror_fish.hdr

mirror_fish.hdr:	mirror.oct
	rad -v  fish  mirror.rif

### End  mirror-fish  tests

### Reference and test for    mist view   def ###

test-mist-def:	ref/mist_def.hdr    mist_def.hdr
	$(RDU_PFILT)    mist_def.hdr | $(IMG_CMP) ref/mist_def.hdr -

ref/mist_def.hdr:
	make    mist_def.hdr
	$(RDU_PFILT)    mist_def.hdr > ref/mist_def.hdr

mist_def.hdr:	mist.rif
	rad -N $(NPROC) mist.rif

### End    mist-def   tests

### Octree for  prism1 ###

prism1.oct:	prism1.rif
	rad -v 0  prism1.rif

### Reference and test for  prism1 view  fish ###

test-prism1-fish:	ref/prism1_fish.hdr  prism1_fish.hdr
	$(RDU_PFILT)  prism1_fish.hdr | $(IMG_CMP) ref/prism1_fish.hdr -

ref/prism1_fish.hdr:
	make  prism1_fish.hdr
	$(RDU_PFILT)  prism1_fish.hdr > ref/prism1_fish.hdr

prism1_fish.hdr:	prism1.oct
	rad -v  fish  prism1.rif

### End  prism1-fish  tests

### Octree for  prism2 ###

prism2.oct:	prism2.rif
	rad -v 0  prism2.rif

### Reference and test for  prism2 view  fish ###

test-prism2-fish:	ref/prism2_fish.hdr  prism2_fish.hdr
	$(RDU_PFILT)  prism2_fish.hdr | $(IMG_CMP) ref/prism2_fish.hdr -

ref/prism2_fish.hdr:
	make  prism2_fish.hdr
	$(RDU_PFILT)  prism2_fish.hdr > ref/prism2_fish.hdr

prism2_fish.hdr:	prism2.oct
	rad -v  fish  prism2.rif

### End  prism2-fish  tests

### Octree for   tfunc ###

tfunc.oct:	tfunc.rif
	rad -v 0   tfunc.rif

### Reference and test for   tfunc view   def ###

test-tfunc-def:	ref/tfunc_def.hdr   tfunc_def.hdr
	$(RDU_PFILT)   tfunc_def.hdr | $(IMG_CMP) ref/tfunc_def.hdr -

ref/tfunc_def.hdr:
	make   tfunc_def.hdr
	$(RDU_PFILT)   tfunc_def.hdr > ref/tfunc_def.hdr

tfunc_def.hdr:	tfunc.oct
	rad -v   def   tfunc.rif

### End   tfunc-def   tests

### Reference and test for   tfunc view  fish ###

test-tfunc-fish:	ref/tfunc_fish.hdr   tfunc_fish.hdr
	$(RDU_PFILT)   tfunc_fish.hdr | $(IMG_CMP) ref/tfunc_fish.hdr -

ref/tfunc_fish.hdr:
	make   tfunc_fish.hdr
	$(RDU_PFILT)   tfunc_fish.hdr > ref/tfunc_fish.hdr

tfunc_fish.hdr:	tfunc.oct
	rad -v  fish   tfunc.rif

### End   tfunc-fish  tests

### Octree for   trans ###

trans.oct:	trans.rif
	rad -v 0   trans.rif

### Reference and test for   trans view   def ###

test-trans-def:	ref/trans_def.hdr   trans_def.hdr
	$(RDU_PFILT)   trans_def.hdr | $(IMG_CMP) ref/trans_def.hdr -

ref/trans_def.hdr:
	make   trans_def.hdr
	$(RDU_PFILT)   trans_def.hdr > ref/trans_def.hdr

trans_def.hdr:	trans.oct
	rad -v   def   trans.rif

### End   trans-def   tests

### Reference and test for   trans view  fish ###

test-trans-fish:	ref/trans_fish.hdr   trans_fish.hdr
	$(RDU_PFILT)   trans_fish.hdr | $(IMG_CMP) ref/trans_fish.hdr -

ref/trans_fish.hdr:
	make   trans_fish.hdr
	$(RDU_PFILT)   trans_fish.hdr > ref/trans_fish.hdr

trans_fish.hdr:	trans.oct
	rad -v  fish   trans.rif

### End   trans-fish  tests

### Octree for  trans2 ###

trans2.oct:	trans2.rif
	rad -v 0  trans2.rif

### Reference and test for  trans2 view   def ###

test-trans2-def:	ref/trans2_def.hdr  trans2_def.hdr
	$(RDU_PFILT)  trans2_def.hdr | $(IMG_CMP) ref/trans2_def.hdr -

ref/trans2_def.hdr:
	make  trans2_def.hdr
	$(RDU_PFILT)  trans2_def.hdr > ref/trans2_def.hdr

trans2_def.hdr:	trans2.oct
	rad -v   def  trans2.rif

### End  trans2-def   tests

### Reference and test for  trans2 view  fish ###

test-trans2-fish:	ref/trans2_fish.hdr  trans2_fish.hdr
	$(RDU_PFILT)  trans2_fish.hdr | $(IMG_CMP) ref/trans2_fish.hdr -

ref/trans2_fish.hdr:
	make  trans2_fish.hdr
	$(RDU_PFILT)  trans2_fish.hdr > ref/trans2_fish.hdr

trans2_fish.hdr:	trans2.oct
	rad -v  fish  trans2.rif

### End  trans2-fish  tests

### Reference and test for  trans2 view   win ###

test-trans2-win:	ref/trans2_win.hdr  trans2_win.hdr
	$(RDU_PFILT)  trans2_win.hdr | $(IMG_CMP) ref/trans2_win.hdr -

ref/trans2_win.hdr:
	make  trans2_win.hdr
	$(RDU_PFILT)  trans2_win.hdr > ref/trans2_win.hdr

trans2_win.hdr:	trans2.oct
	rad -v   win  trans2.rif

### End  trans2-win   tests

### Octree for patterns ###

patterns.oct:	patterns.rif
	rad -v 0 patterns.rif

### Reference and test for patterns view   def ###

test-patterns-def:	ref/patterns_def.hdr patterns_def.hdr
	$(RDU_PFILT) patterns_def.hdr | $(IMG_CMP) ref/patterns_def.hdr -

ref/patterns_def.hdr:
	make patterns_def.hdr
	$(RDU_PFILT) patterns_def.hdr > ref/patterns_def.hdr

patterns_def.hdr:	patterns.oct
	rad -v   def patterns.rif

### End patterns-def   tests

### Reference and test for patterns view   plan ###

test-patterns-plan:	ref/patterns_plan.hdr patterns_plan.hdr
	$(RDU_PFILT) patterns_plan.hdr | $(IMG_CMP) ref/patterns_plan.hdr -

ref/patterns_plan.hdr:
	make patterns_plan.hdr
	$(RDU_PFILT) patterns_plan.hdr > ref/patterns_plan.hdr

patterns_plan.hdr:	patterns.oct
	rad -v   plan patterns.rif

### End patterns-plan   tests

### Octree for  mixtex ###

mixtex.oct:	mixtex.rif
	rad -v 0 -N $(NPROC) mixtex.rif

### Reference and test for  mixtex view   def ###

test-mixtex-def:	ref/mixtex_def.hdr  mixtex_def.hdr
	$(RDU_PFILT)  mixtex_def.hdr | $(IMG_CMP) ref/mixtex_def.hdr -

ref/mixtex_def.hdr:
	make  mixtex_def.hdr
	$(RDU_PFILT)  mixtex_def.hdr > ref/mixtex_def.hdr

mixtex_def.hdr:	mixtex.oct
	rad -v def -N $(NPROC) mixtex.rif

### End  mixtex-def   tests

### Reference and test for  mixtex view  fish ###

test-mixtex-fish:	ref/mixtex_fish.hdr  mixtex_fish.hdr
	$(RDU_PFILT)  mixtex_fish.hdr | $(IMG_CMP) ref/mixtex_fish.hdr -

ref/mixtex_fish.hdr:
	make  mixtex_fish.hdr
	$(RDU_PFILT)  mixtex_fish.hdr > ref/mixtex_fish.hdr

mixtex_fish.hdr:	mixtex.oct
	rad -v  fish  mixtex.rif

### End  mixtex-fish  tests

### Reference and test for  mixtex view  plan ###

test-mixtex-plan:	ref/mixtex_plan.hdr  mixtex_plan.hdr
	$(RDU_PFILT)  mixtex_plan.hdr | $(IMG_CMP) ref/mixtex_plan.hdr -

ref/mixtex_plan.hdr:
	make  mixtex_plan.hdr
	$(RDU_PFILT)  mixtex_plan.hdr > ref/mixtex_plan.hdr

mixtex_plan.hdr:	mixtex.oct
	rad -v  plan  mixtex.rif

### End  mixtex-plan  tests

### Reference and test for  mixtex view rplan ###

test-mixtex-rplan:	ref/mixtex_rplan.hdr  mixtex_rplan.hdr
	$(RDU_PFILT)  mixtex_rplan.hdr | $(IMG_CMP) ref/mixtex_rplan.hdr -

ref/mixtex_rplan.hdr:
	make  mixtex_rplan.hdr
	$(RDU_PFILT)  mixtex_rplan.hdr > ref/mixtex_rplan.hdr

mixtex_rplan.hdr:	mixtex.oct
	rad -v rplan  mixtex.rif

### End  mixtex-rplan tests
