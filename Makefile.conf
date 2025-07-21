PYTHON_SETUP	=	python setup.py
BRANCH	=	`pwd | tail -c 5`
SITE	=	$(notdir $(CURDIR))

isort:
	find . -name '*.py' | xargs isort --profile django

spell:
	-find . -name '*.py' | xargs codespell -w

black: isort
	find . -name '*.py' | xargs black -l 119

djhtml:
	djhtml apps/ templates/

check:
	python manage.py check

commit:	_commit restart

_commit: permissions spell isort black check restart
	git add apps/
	git commit -a
	git push origin ${BRANCH}

permissions:
	setfacl --set-file=configs/acls.txt -R apps/
	setfacl --set-file=configs/acls.txt -R phas${BRANCH}/

restart: permissions
	sudo systemctl restart ${SITE}

FORCE:
