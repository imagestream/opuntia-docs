# opuntia-docs
Documentation for Opuntia 

To build the english version of the site use the following command.

make html

To build the spanish version of the site use the following command.

make -e SPHINXOPTS="-D language='es'" html

To update the po files, first generate new gettext from the rst files. 

sphinx-build -b gettext . _build/gettext

Then use sphinx-intl to update the po files. 

sphinx-intl update -p _build/gettext -l es

