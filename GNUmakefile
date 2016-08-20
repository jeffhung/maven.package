

.PHONY: all
all: help

.PHONY: help
help:
	@echo "Usage: make [ TARGET ... ]";
	@echo "";
	@echo "TARGET:";
	@echo "";
	@echo "  help      - show this help message";
	@echo "  rpm       - package as RPM file (in builder)";
	@echo "  deb       - package as DEB file (in builder)";
	@echo "  run       - install and run the package (in runner)";
	@echo "  stop      - stop builder/runner instances";
	@echo "  clean     - delete all generated files";
	@echo "  distclean - delete all generated files and caches";
	@echo "";
	@echo "Default TARGET is 'help'.";


.PHONY: clean
clean: stop
	[ -e rpm.make ] && $(MAKE) -f rpm.make rpm-clean;

.PHONY: distclean
distclean: clean
	rm -f rpm.make;

.PHONY: rpm
rpm: roles rpm.make
	vagrant up --no-provision
	vagrant provision --provision-with builder;
	vagrant ssh -c 'make -C /vagrant maven.rpm;';

.PHONY: run
run:
	vagrant up --no-provision
	vagrant provision --provision-with runner;

.PHONY: stop
stop:
	vagrant destroy --force;

.PHONY: maven.rpm
maven.rpm: export RPM_DISTS_DIR := files
maven.rpm: export RPM_NAME      := maven
maven.rpm: export RPM_VERSION   := 2.2.1
maven.rpm: export RPM_PACKER    :=
maven.rpm: export RPM_SOURCE0   := files/apache-maven-2.2.1-bin.tar.gz
maven.rpm: rpm.make
	mkdir -p $(RPM_DISTS_DIR);
	$(MAKE) -f rpm.make rpm-pack;

rpm.make:
	curl -L -o rpm.make https://bit.ly/rpm-make;

.PHONY: roles
roles: roles/jeffhung.maven/tasks/main.yml \
       roles/jeffhung.localrepo/tasks/main.yml \
       roles/jeffhung.java-se/tasks/main.yml

roles/jeffhung.maven/tasks/main.yml:
	ansible-galaxy install --roles-path=roles jeffhung.maven

roles/jeffhung.localrepo/tasks/main.yml:
	ansible-galaxy install --roles-path=roles jeffhung.localrepo

roles/jeffhung.java-se/tasks/main.yml:
	ansible-galaxy install --roles-path=roles jeffhung.java-se

