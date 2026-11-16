# Issue

**Title:** build: fix the compilation of poisson_integrator

## Description

## Summary

I get errors like "undefined reference to `colvarmodule::cv_width'" when compiling poisson_integrator from colvartools. Changing the C++ standard from C++11 to C++17 in src/Makefile solves the issue.

## Related issue(s)

When I was trying to compile poisson_integrator, I got a lot of errors about undefined references. Here's the log

```
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvaratoms.o colvaratoms.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvaratoms_gpu.o colvaratoms_gpu.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvarbias_abf.o colvarbias_abf.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvarbias_abmd.o colvarbias_abmd.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvarbias_alb.o colvarbias_alb.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvarbias.o colvarbias.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvarbias_histogram.o colvarbias_histogram.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvarbias_histogram_reweight_amd.o colvarbias_histogram_reweight_amd.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvarbias_meta.o colvarbias_meta.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvarbias_opes.o colvarbias_opes.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvarbias_restraint.o colvarbias_restraint.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvarcomp_alchlambda.o colvarcomp_alchlambda.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvarcomp_angles.o colvarcomp_angles.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvarcomp_apath.o colvarcomp_apath.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvarcomp_combination.o colvarcomp_combination.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvarcomp_coordnums.o colvarcomp_coordnums.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvarcomp.o colvarcomp.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvarcomp_distances.o colvarcomp_distances.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvarcomp_gpath.o colvarcomp_gpath.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvarcomp_neuralnetwork.o colvarcomp_neuralnetwork.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvarcomp_protein.o colvarcomp_protein.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvarcomp_rotations.o colvarcomp_rotations.cpp
In file included from colvarcomp_coordnums.cpp:15:
colvarcomp_coordnums.h: In static member function ‘static colvarmodule::real colvar::coordnum::switching_function(const colvarmodule::real&, colvarmodule::real&, int, int, colvarmodule::real)’:
colvarcomp_coordnums.h:192:6: warning: ‘if constexpr’ only available with ‘-std=c++17’ or ‘-std=gnu++17’ [-Wc++17-extensions]
  192 |   if constexpr ((static_en > 0) && (static_ed > 0)) {
      |      ^~~~~~~~~
colvarcomp_coordnums.h:199:6: warning: ‘if constexpr’ only available with ‘-std=c++17’ or ‘-std=gnu++17’ [-Wc++17-extensions]
  199 |   if constexpr ((static_en > 0) && (static_ed > 0)) {
      |      ^~~~~~~~~
colvarcomp_coordnums.h:200:8: warning: ‘if constexpr’ only available with ‘-std=c++17’ or ‘-std=gnu++17’ [-Wc++17-extensions]
  200 |     if constexpr (static_ed_is_2en) {
      |        ^~~~~~~~~
colvarcomp_coordnums.h:202:10: warning: ‘if constexpr’ only available with ‘-std=c++17’ or ‘-std=gnu++17’ [-Wc++17-extensions]
  202 |       if constexpr (static_en == 2) {
      |          ^~~~~~~~~
colvarcomp_coordnums.h:228:6: warning: ‘if constexpr’ only available with ‘-std=c++17’ or ‘-std=gnu++17’ [-Wc++17-extensions]
  228 |   if constexpr (static_ed_is_2en) {
      |      ^~~~~~~~~
colvarcomp_coordnums.h:256:8: warning: ‘if constexpr’ only available with ‘-std=c++17’ or ‘-std=gnu++17’ [-Wc++17-extensions]
  256 |     if constexpr (static_ed_is_2en) {
      |        ^~~~~~~~~
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvarcomp_torchann.o colvarcomp_torchann.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvarcomp_volmaps.o colvarcomp_volmaps.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvar.o colvar.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvardeps.o colvardeps.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvar_gpu_calc.o colvar_gpu_calc.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvar_gpu_support.o colvar_gpu_support.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvargrid.o colvargrid.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvargrid_integrate.o colvargrid_integrate.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvarmodule.o colvarmodule.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvar_neuralnetworkcompute.o colvar_neuralnetworkcompute.cpp
In file included from colvarcomp_protein.cpp:16:
colvarcomp_coordnums.h: In static member function ‘static colvarmodule::real colvar::coordnum::switching_function(const colvarmodule::real&, colvarmodule::real&, int, int, colvarmodule::real)’:
colvarcomp_coordnums.h:192:6: warning: ‘if constexpr’ only available with ‘-std=c++17’ or ‘-std=gnu++17’ [-Wc++17-extensions]
  192 |   if constexpr ((static_en > 0) && (static_ed > 0)) {
      |      ^~~~~~~~~
colvarcomp_coordnums.h:199:6: warning: ‘if constexpr’ only available with ‘-std=c++17’ or ‘-std=gnu++17’ [-Wc++17-extensions]
  199 |   if constexpr ((static_en > 0) && (static_ed > 0)) {
      |      ^~~~~~~~~
colvarcomp_coordnums.h:200:8: warning: ‘if constexpr’ only available with ‘-std=c++17’ or ‘-std=gnu++17’ [-Wc++17-extensions]
  200 |     if constexpr (static_ed_is_2en) {
      |        ^~~~~~~~~
colvarcomp_coordnums.h:202:10: warning: ‘if constexpr’ only available with ‘-std=c++17’ or ‘-std=gnu++17’ [-Wc++17-extensions]
  202 |       if constexpr (static_en == 2) {
      |          ^~~~~~~~~
colvarcomp_coordnums.h:228:6: warning: ‘if constexpr’ only available with ‘-std=c++17’ or ‘-std=gnu++17’ [-Wc++17-extensions]
  228 |   if constexpr (static_ed_is_2en) {
      |      ^~~~~~~~~
colvarcomp_coordnums.h:256:8: warning: ‘if constexpr’ only available with ‘-std=c++17’ or ‘-std=gnu++17’ [-Wc++17-extensions]
  256 |     if constexpr (static_ed_is_2en) {
      |        ^~~~~~~~~
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvarparams.o colvarparams.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvarparse.o colvarparse.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvarproxy.o colvarproxy.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvarproxy_gpu.o colvarproxy_gpu.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvarproxy_io.o colvarproxy_io.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvarproxy_replicas.o colvarproxy_replicas.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvarproxy_system.o colvarproxy_system.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvarproxy_tcl.o colvarproxy_tcl.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvarproxy_volmaps.o colvarproxy_volmaps.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvar_rotation_derivative.o colvar_rotation_derivative.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvarscript_commands_bias.o colvarscript_commands_bias.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvarscript_commands_colvar.o colvarscript_commands_colvar.cpp
In file included from colvar.cpp:24:
colvarcomp_coordnums.h: In static member function ‘static colvarmodule::real colvar::coordnum::switching_function(const colvarmodule::real&, colvarmodule::real&, int, int, colvarmodule::real)’:
colvarcomp_coordnums.h:192:6: warning: ‘if constexpr’ only available with ‘-std=c++17’ or ‘-std=gnu++17’ [-Wc++17-extensions]
  192 |   if constexpr ((static_en > 0) && (static_ed > 0)) {
      |      ^~~~~~~~~
colvarcomp_coordnums.h:199:6: warning: ‘if constexpr’ only available with ‘-std=c++17’ or ‘-std=gnu++17’ [-Wc++17-extensions]
  199 |   if constexpr ((static_en > 0) && (static_ed > 0)) {
      |      ^~~~~~~~~
colvarcomp_coordnums.h:200:8: warning: ‘if constexpr’ only available with ‘-std=c++17’ or ‘-std=gnu++17’ [-Wc++17-extensions]
  200 |     if constexpr (static_ed_is_2en) {
      |        ^~~~~~~~~
colvarcomp_coordnums.h:202:10: warning: ‘if constexpr’ only available with ‘-std=c++17’ or ‘-std=gnu++17’ [-Wc++17-extensions]
  202 |       if constexpr (static_en == 2) {
      |          ^~~~~~~~~
colvarcomp_coordnums.h:228:6: warning: ‘if constexpr’ only available with ‘-std=c++17’ or ‘-std=gnu++17’ [-Wc++17-extensions]
  228 |   if constexpr (static_ed_is_2en) {
      |      ^~~~~~~~~
colvarcomp_coordnums.h:256:8: warning: ‘if constexpr’ only available with ‘-std=c++17’ or ‘-std=gnu++17’ [-Wc++17-extensions]
  256 |     if constexpr (static_ed_is_2en) {
      |        ^~~~~~~~~
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvarscript_commands.o colvarscript_commands.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvarscript.o colvarscript.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvars_memstream.o colvars_memstream.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvartypes.o colvartypes.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o colvarvalue.o colvarvalue.cpp
g++ -std=c++11 -pedantic -g -O2 -fPIC -Wall  -I/home/hanatok/HDD/Documents/git/colvars/src  -c -o nr_jacobi.o nr_jacobi.cpp
ar -rv libcolvars.a colvaratoms.o colvaratoms_gpu.o colvarbias_abf.o colvarbias_abmd.o colvarbias_alb.o colvarbias.o colvarbias_histogram.o colvarbias_histogram_reweight_amd.o colvarbias_meta.o colvarbias_opes.o colvarbias_restraint.o colvarcomp_alchlambda.o colvarcomp_angles.o colvarcomp_apath.o colvarcomp_combination.o colvarcomp_coordnums.o colvarcomp.o colvarcomp_distances.o colvarcomp_gpath.o colvarcomp_neuralnetwork.o colvarcomp_protein.o colvarcomp_rotations.o colvarcomp_torchann.o colvarcomp_volmaps.o colvar.o colvardeps.o colvar_gpu_calc.o colvar_gpu_support.o colvargrid.o colvargrid_integrate.o colvarmodule.o colvar_neuralnetworkcompute.o colvarparams.o colvarparse.o colvarproxy.o colvarproxy_gpu.o colvarproxy_io.o colvarproxy_replicas.o colvarproxy_system.o colvarproxy_tcl.o colvarproxy_volmaps.o colvar_rotation_derivative.o colvarscript_commands_bias.o colvarscript_commands_colvar.o colvarscript_commands.o colvarscript.o colvars_memstream.o colvartypes.o colvarvalue.o nr_jacobi.o
ar: 正在创建 libcolvars.a
a - colvaratoms.o
a - colvaratoms_gpu.o
a - colvarbias_abf.o
a - colvarbias_abmd.o
a - colvarbias_alb.o
a - colvarbias.o
a - colvarbias_histogram.o
a - colvarbias_histogram_reweight_amd.o
a - colvarbias_meta.o
a - colvarbias_opes.o
a - colvarbias_restraint.o
a - colvarcomp_alchlambda.o
a - colvarcomp_angles.o
a - colvarcomp_apath.o
a - colvarcomp_combination.o
a - colvarcomp_coordnums.o
a - colvarcomp.o
a - colvarcomp_distances.o
a - colvarcomp_gpath.o
a - colvarcomp_neuralnetwork.o
a - colvarcomp_protein.o
a - colvarcomp_rotations.o
a - colvarcomp_torchann.o
a - colvarcomp_volmaps.o
a - colvar.o
a - colvardeps.o
a - colvar_gpu_calc.o
a - colvar_gpu_support.o
a - colvargrid.o
a - colvargrid_integrate.o
a - colvarmodule.o
a - colvar_neuralnetworkcompute.o
a - colvarparams.o
a - colvarparse.o
a - colvarproxy.o
a - colvarproxy_gpu.o
a - colvarproxy_io.o
a - colvarproxy_replicas.o
a - colvarproxy_system.o
a - colvarproxy_tcl.o
a - colvarproxy_volmaps.o
a - colvar_rotation_derivative.o
a - colvarscript_commands_bias.o
a - colvarscript_commands_colvar.o
a - colvarscript_commands.o
a - colvarscript.o
a - colvars_memstream.o
a - colvartypes.o
a - colvarvalue.o
a - nr_jacobi.o
make[1]: 离开目录“/home/hanatok/HDD/Documents/git/colvars/src”
g++ -o poisson_integrator -Wall -O2 -I../src poisson_integrator.o ../src/libcolvars.a
/usr/bin/ld.bfd: ../src/libcolvars.a(colvarmodule.o): in function `colvarmodule::parse_colvars(std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > const&)':
/home/hanatok/HDD/Documents/git/colvars/src/colvarmodule.cpp:610:(.text+0x7275): undefined reference to `colvarmodule::line_marker'
/usr/bin/ld.bfd: ../src/libcolvars.a(colvarmodule.o): in function `colvarmodule::print_total_forces_errning(bool)':
/home/hanatok/HDD/Documents/git/colvars/src/colvarmodule.cpp:1904:(.text+0x8e11): undefined reference to `colvarmodule::line_marker'
/usr/bin/ld.bfd: ../src/libcolvars.a(colvarmodule.o): in function `colvarmodule::colvarmodule(colvarproxy*)':
/home/hanatok/HDD/Documents/git/colvars/src/colvarmodule.cpp:130:(.text+0xf5c0): undefined reference to `colvarmodule::line_marker'
/usr/bin/ld.bfd: ../src/libcolvars.a(colvarmodule.o): in function `colvarmodule::parse_biases(std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > const&)':
/home/hanatok/HDD/Documents/git/colvars/src/colvarmodule.cpp:716:(.text+0x138e3): undefined reference to `colvarmodule::line_marker'
/usr/bin/ld.bfd: /home/hanatok/HDD/Documents/git/colvars/src/colvarmodule.cpp:731:(.text+0x13c47): undefined reference to `colvarmodule::line_marker'
/usr/bin/ld.bfd: ../src/libcolvars.a(colvarmodule.o):/home/hanatok/HDD/Documents/git/colvars/src/colvarmodule.cpp:377: more undefined references to `colvarmodule::line_marker' follow
/usr/bin/ld.bfd: ../src/libcolvars.a(colvarbias_abmd.o): in function `colvarbias_abmd::write_traj_label(std::ostream&)':
/home/hanatok/HDD/Documents/git/colvars/src/colvarbias_abmd.cpp:122:(.text+0x384): undefined reference to `colvarmodule::cv_width'
/usr/bin/ld.bfd: ../src/libcolvars.a(colvarbias_alb.o): in function `colvarbias_alb::write_traj_label(std::ostream&)':
/home/hanatok/HDD/Documents/git/colvars/src/colvarbias_alb.cpp:383:(.text+0x557): undefined reference to `colvarmodule::cv_width'
/usr/bin/ld.bfd: ../src/libcolvars.a(colvarbias_restraint.o): in function `colvarbias_restraint_centers_moving::write_traj_label(std::ostream&)':
/home/hanatok/HDD/Documents/git/colvars/src/colvarbias_restraint.cpp:493:(.text+0x19c6): undefined reference to `colvarmodule::cv_width'
/usr/bin/ld.bfd: ../src/libcolvars.a(colvar.o): in function `colvar::write_traj_label(std::ostream&)':
/home/hanatok/HDD/Documents/git/colvars/src/colvar.cpp:2750:(.text+0x5c5e): undefined reference to `colvarmodule::cv_width'
/usr/bin/ld.bfd: ../src/libcolvars.a(colvar.o): in function `colvar::calc_runave()':
/home/hanatok/HDD/Documents/git/colvars/src/colvar.cpp:3157:(.text+0x177ec): undefined reference to `colvarmodule::cv_width'
```

## Affected engine(s)

N/A

## Author(s)

@HanatoK

## Licensing

By submitting this pull request, I agree that my contribution will be included in Colvars repository and redistributed under the GNU Lesser General Public License (LGPL) version 3.

*Note:* NAMD, VMD and TinkerHP follow more restrictive licenses, e.g. the [UIUC license](https://www.ks.uiuc.edu/Research/namd/license.html).  However, the terms of the LGPLv3 remain in effect for the copies of the Colvars code distributed alongside those packages.

## Artificial Intelligence (AI)

N/A

## Further information / links

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4b7604b79a60`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.