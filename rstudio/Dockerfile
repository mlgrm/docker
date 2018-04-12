# add some packages to ocpu/rstudio to run svy
FROM opencpu/rstudio:latest

MAINTAINER mlgrm version: 0.01

RUN 	apt-get update
RUN	apt-get upgrade -y
RUN	apt-get install -y \
		wget \
		r-cran-rcurl \
		r-cran-rjava \
		r-cran-jsonlite \
		r-cran-dbi \
		r-cran-rpostgresql 

RUN 	echo 	"r <- getOption('repos');" \
		"r['CRAN'] <- 'http://cran.rstudio.com';" \
		"options(repos = r);" > ~/.Rprofile

RUN	Rscript -e "install.packages(c('xml2','xlsx','devtools'))"
RUN	Rscript -e "install.packages(c('dplyr','dbplyr'))"
RUN	Rscript -e "install.packages('shiny')"
RUN	Rscript -e "devtools::install_github('hrbrmstr/jsonview')"
RUN	Rscript -e "devtools::install_github('jennybc/googlesheets')"
RUN	Rscript -e "devtools::install_github('mlgrm/svyr2')"

RUN	wget https://download3.rstudio.org/ubuntu-12.04/x86_64/shiny-server-1.5.6.875-amd64.deb \
    && dpkg -i shiny-server-1.5.6.875-amd64.deb

# httr oauth doesn't work "in band" on rstudio server, so set the default to 
# "TRUE" for httr out of band.  this means that when you run oauth, e.g. with 
# googlesheets, you'll need to copy and paste the auth token into rstudio
RUN echo "\noptions(httr_oob_default=TRUE)\n" >> /etc/R/Rprofile.site 

RUN Rscript -e "install.packages('googledrive')"

EXPOSE 8787
EXPOSE 8004
EXPOSE 3838

CMD	/usr/lib/rstudio-server/bin/rserver && apachectl -DFOREGROUND
