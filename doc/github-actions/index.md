# $BL?L>5,Ls(B

$B$3$N%I%-%e%a%s%H$*$h$S(B GitHub Actions $B<BAu$G$O!"0J2<$NL>>N$rMQ$$$^$9!#(B

## $B%o!<%/%U%m!<$N%U%!%$%kL>$K$D$$$F(B

$B%j%]%8%H%j(B/.github/workflow/*.yml $B$H$7$F@_CV$9$k%o!<%/%U%m!<$NL?L>5,Ls$G$9!#(B

* test.yml

	* git push $B$d(B pull request $B$r7@5!$K!"%F%9%H$r<B9T$9$k%o!<%/%U%m!<$NL>>N(B

* rpm.RPM$BL>>N(B.yml

	* source RPM $B$*$h$S(B binary RPM $B$r@8@.$9$k%o!<%/%U%m!<$NL>>N(B

* docker.Docker$B%$%a!<%8L>>N(B.yml

	* Docker $B%$%a!<%8$r@8@.$9$k%o!<%/%U%m!<$NL>>N(B

* $BBP>]%o!<%/%U%m!<%U%!%$%kL>(B.invoke.yml

$B0z?t$D$-$N(B repository_dispatch $B$r(B Web UI $B$G5/F0$9$kMQ(B

	* workflow_dispatch $B$9$J$o$A(B WEb UI $B$r7@5!$K!"(B
	  $BB>$N%o!<%/%U%m!<$r(B repository_dispatch $B$G5/F0$9$k%o!<%/%U%m!<$O!"(B
	  $B5/F0BP>]$N%U%!%$%kL>$K(B .invoke $B$rIU2C$7$?L>>N$H$7$^$9(B

## $B%o!<%/%U%m!<$N(B name $B$K$D$$$F(B

$B%o!<%/%U%m!<$N(B name $BMs$O!"(BGitHub Actions $B$N(B Web UI $B$GI=<($5$l$k9`L\$G$9!#(B
$B0J2<$N$h$&$K@_Dj$7$^$9(B

* $BF0;l$G3+;O$9$kL?NaJ8(B

	* on workflow_dispatch $B$r4^$`(B ($B$9$J$o$A<jF0(B Web UI $B$G5/F0$G$-$k(B) $B%o!<%/%U%m!<$GMQ$$$^$9(B

	* $BNc(B

		* create pip-glibc RPM
		* create pip-glibc Docker image

* $BL>;l6g(B

	* on workflow_dispatch $B$r4^$^$J$$!"FbIt<BAuMQES$N%o!<%/%U%m!<$GMQ$$$^$9(B

	* $BNc(B

		* pip-glibc RPM
		* pip-glibc Docker image

## $B%o!<%/%U%m!<$GMxMQ$9$kJQ?t$NL>>N(B

$B!V!A(Bs $B!W$N$h$&$KJ#?t7A$N>l9g$O!"(B
$B%j%9%H$J$$$7%+%s%^6h@Z$j$NJ8;zNs7A<0$GJ#?t$NMWAG$rJ];}$7$^$9(B

* distro

	* centos7 $B$J$$$7(B centos8 $B$NCM$r$H$j$^$9(B

* platform

	* Docker $B$N<B9T4D6-L>$G$9(B

	* linux/amd64 $B$J$$$7(B linux/arm64 $B$NCM$r$H$j$^$9(B

* arch

	* platform $B$+$i!V(Blinux/$B!W$r:o=|$7$?L>>N$G$9(B

	* amd64 $B$J$$$7(B arm64 $B$NCM$r$H$j$^$9(B

* archtype

	* Docker $B$N%$%a!<%8<oJL$G$9(B

	* arch $B$G$H$j$&$kCM$K2C$(!"$5$i$K!V(Bmultiarch$B!W$H$$$&CM$r$H$j$^$9(B

# $B<BAu$7$?%o!<%/%U%m!<(B

$B0J2<!V%j%]%8%H%jL>(B:$B%o!<%/%U%m!<%U%!%$%kL>!W$H$$$&7A<0$G5-=R$7$^$9!#(B

client_payload $B$O(B repository_dispatch $B$G<u$1<h$kF~NO0z?t$G$9!#(B

## PiP-glibc $B%j%]%8%H%j(B

* PiP-glibc:test.yml

	* PiP-glibc $B$N%F%9%H(B ($B<B:]$K$O%S%k%I$N$_(B)

	* push, pull-request, workflow_dispatch $B$G5/F0$7$^$9(B

* PiP-glibc:rpm.pip-glibc.yml

	* PiP-glibc $B$N(B RPM $B$N%S%k%I(B

	* repository_dispatch $B$G5/F0$7$^$9!#(B

	* client_payload $B$O0J2<$NDL$j$G$9(B

		* distros: centos7 centos8
		* archs: amd64 arm64

* PiP-glibc:rpm.pip-glibc.invoke.yml

	* rpm.pip-glibc.yml $B$N8F$S=P$7MQ$G$9(B

	* workflow_dispatch $B$G5/F0$7$^$9(B

* PiP-glibc:docker.pip-glibc.yml

	* repository_dispatch $B$G5/F0$7$^$9!#(B

	* client_payload $B$O0J2<$NDL$j$G$9(B

		* distros: centos7 centos8
		* archtypes: multiarch amd64 arm64

* PiP-glibc:docker.pip-glibc.invoke.yml

	* docker.pip-glibc.yml $B$N8F$S=P$7MQ$G$9(B

	* workflow_dispatch $B$G5/F0$7$^$9(B


## PiP $B%j%]%8%H%j(B

* PiP:test.yml

	* repository_dispatch $B$G5/F0$7$^$9!#(B

	* client_payload $B$O0J2<$NDL$j$G$9(B

		* distros: [centos7, centos8]
		* archs: [amd64, arm64]
		* pip_versions: [2, 3] or [''] if pip_ref != ''
		* pip_ref: commit_hash or ''
		* pip_testsuite_ref: commit_hash or ''
		* dispatch: true or false

* PiP:test.invoke.yml

	* test.yml $B$N8F$S=P$7MQ$G$9(B

	* push, pull-request, workflow_dispatch $B$G5/F0$7$^$9(B

* PiP:rpm.pip.yml

	* repository_dispatch $B$G5/F0$7$^$9!#(B

	* client_payload $B$O0J2<$NDL$j$G$9(B

		* distros: centos7 centos8
		* archs: amd64 arm64
		* pip_versions: 2 3

* PiP:docker.pip-glibc-libpip.yml

	* repository_dispatch $B$G5/F0$7$^$9!#(B

	* client_payload $B$O0J2<$NDL$j$G$9(B

		* distros: centos7 centos8
		* archtypes: multiarch amd64 arm64
		* pip_versions: 2 3

## PiP-gdb $B%j%]%8%H%j(B

* PiP-gdb:test.yml

	* push, pull-request, workflow_dispatch $B$G5/F0$7$^$9(B

* PiP-gdb:rpm.pip-gdb.yml

	* repository_dispatch $B$G5/F0$7$^$9!#(B

	* client_payload $B$O0J2<$NDL$j$G$9(B

		* distros: centos7 centos8
		* archs: amd64 arm64
		* pip_versions: 2 3

* PiP-gdb:docker.pip-glibc-libpip-gdb.yml

	* repository_dispatch $B$G5/F0$7$^$9!#(B

	* client_payload $B$O0J2<$NDL$j$G$9(B

		* distros: centos7 centos8
		* archtypes: multiarch amd64 arm64
		* pip_versions: 2 3

## PiP-Testsuite $B%j%]%8%H%j(B

* PiP-Testsuite:test.invoke.yml

	* push, pull-request, workflow_dispatch $B$G5/F0$7$^$9(B

## PiP-pip $B%j%]%8%H%j(B

* PiP-pip:test.yml

	* push, pull-request, workflow_dispatch $B$G5/F0$7$^$9(B

* PiP-pip:docker.pip-prep.yml

	* push $B$*$h$S(B workflow_dispatch $B$G5/F0$7$^$9(B

* PiP-pip:docker.process-in-process.yml

	* repository_dispatch $B$G5/F0$7$^$9!#(B

	* client_payload $B$O0J2<$NDL$j$G$9(B

		* distros: centos7 centos8

		* pip_versions: 2 3

		* archtypes $B0z?t$O$J$$!#(B
		  $B$3$l$O!"$N8BDj$O(B RPM $B$d(B Docker $B%$%a!<%8$N@8@.$G$OI,MW$@$,(B
		  $B%(%s%I%f!<%6!<8~$1(B Docker $B%$%a!<%8Ds6!$,L\E*$N(B
		  $B$3$N%o!<%/%U%m!<$G$O(B multiarch $B$N$_$NDs6!$G==J,$J$?$a(B

* PiP-pip:docker.process-in-process.invoke.yml

	* workflow_dispatch $B$G5/F0$7$^$9!#(B

	* docker.process-in-process.yml $B$N8F$S=P$7MQ(B

# $B<BAu$7$?%o!<%/%U%m!<4V$N8F$S=P$74X78(B

repository_dispatch $B5!G=$K$h$k!"%o!<%/%U%m!<4V$N8F$S=P$74X78$r<($7$^$9!#(B

$B3g8LFb$O%$%Y%s%HL>$G$9!#(B

## PiP-glibc $B%j%]%8%H%j(B

* PiP-glibc:test.yml
  $B"*(B (pip-glibc-test-ok) $B"*(B PiP-glibc:rpm.pip-glibc.yml

	* $B"((B pull-request $B$N>l9g$O(B dispatch $B$7$J$$(B

* PiP-glibc:rpm.pip-glibc.invoke.yml
  $B"*(B (pip-glibc-rpm-invoke) $B"*(B PiP-glibc:rpm.pip-glibc.yml

* PiP-glibc:rpm.pip-glibc.yml
  $B"*(B (pip-glibc-rpm-built) $B"*(B PiP-glibc:docker.pip-glibc.yml

* PiP-glibc:docker.pip-glibc.invoke.yml
  $B"*(B (pip-glibc-docker-invoke) $B"*(B PiP-glibc:docker.pip-glibc.yml

* PiP-glibc:docker.pip-glibc.yml
  $B"*(B (pip-glibc-built) $B"*(B PiP:test.yml

## PiP $B%j%]%8%H%j(B

* PiP:update.yml
  $B"*(B (pip-update) $B"*(B PiP:test.yml

	* $B"((B pull-request $B$N>l9g$O(B dispatch: false $B$rEO$9(B

* PiP:test.invoke.yml
  $B"*(B (pip-test-invoke) $B"*(B PiP:test.yml

* PiP:test.yml
  $B"*(B (pip-test-ok) $B"*(B PiP:rpm.pip.yml

* PiP:rpm.process-in-process.yml
  $B"*(B (pip-rpm-built) $B"*(B PiP:docker.pip-glibc-libpip

* PiP:docker.pip-glibc-libpip
  $B"*(B (pip-built) $B"*(B PiP-gdb:test.yml

## PiP-Testsuite $B%j%]%8%H%j(B

* PiP-Testsuite:test.invoke.yml
  $B"*(B (pip-testsuite-update) $B"*(B PiP:test.yml

	* $B"((B pull-request $B$N>l9g$O(B dispatch $B$7$J$$(B

## PiP-Testsuite $B%j%]%8%H%j(B

* PiP-Testsuite:update.yml
  $B"*(B (pip-testsuite-update) $B"*(B PiP:test.yml

	* $B"((B $B>o$K(B dispatch: false $B$rEO$9(B

## PiP-gdb $B%j%]%8%H%j(B

* PiP-gdb:test.yml
  $B"*(B (pip-gdb-test-ok) $B"*(B PiP:rpm.pip.yml

	* $B"((B pull-request $B$N>l9g$O(B dispatch $B$7$J$$(B

* PiP-gdb:test.yml
  $B"*(B (pip-gdb-test-ok) $B"*(B PiP-pip:test.yml

	* $B"((B pull-request $B$N>l9g$O(B dispatch $B$7$J$$(B

* PiP-gdb:rpm.pip-gdb.yml
  $B"*(B (pip-gdb-rpm-built) $B"*(B PiP-gdb:docker.pip-glibc-libpip-gdb.yml

* PiP-gdb:docker.pip-glibc-libpip-gdb.yml

## PiP-pip $B%j%]%8%H%j(B

* PiP-pip:test.yml
  $B"*(B (pip-pip-test-ok) $B"*(B PiP-pip:docker.process-in-process.yml

	* $B"((B pull-request $B$N>l9g$O(B dispatch $B$7$J$$(B

* PiP-pip:docker.pip-prep.yml
  $B"*(B (pip-prep-docker-built) $B"*(B PiP-glibc:docker.pip-glibc.yml

