# README

## Setup

1. Install R and RStduio for MacOS.

    ```bash
    brew install --cask r
    brew install --cask rstudio
    ```

1. Create a dedicated folder for the installed R packages and let R know about it.

    ```bash
    export RLIBS_PATH=/Users/$(whoami)/tools/rlibs
    mkdir -p $RLIBS_PATH
    echo ".libPaths(\"${RLIBS_PATH}\")" >> ~/.Rprofile
    ```

1. Install `knitr`, `DiagrammeR`, and `rmarkdown` packages.

    ```bash
    # knitr
    mycmd=install.packages(\"knitr\", lib=\"${RLIBS_PATH}\", repos=\"https://cloud.r-project.org\")
    Rscript -e $mycmd

    # DiagrammeR
    mycmd=install.packages(\"DiagrammeR\", lib=\"${RLIBS_PATH}\", repos=\"https://cloud.r-project.org\")
    Rscript -e $mycmd

    # rmarkdown
    mycmd=install.packages(\"rmarkdown\", lib=\"${RLIBS_PATH}\", repos=\"https://cloud.r-project.org\")
    Rscript -e $mycmd

    # kableExtra
    mycmd=install.packages(\"kableExtra\", lib=\"${RLIBS_PATH}\", repos=\"https://cloud.r-project.org\")
    Rscript -e $mycmd
    ```

1. Upgrade `mermaid.js` bundled with the `DiagrammeR` package. Note that we will need `mermaid.js` for creating drawings programmatically.

    ```bash
    # Remove original copies
    rm ${RLIBS_PATH}/DiagrammeR/htmlwidgets/lib/mermaid/dist/mermaid.slim.min.js
    rm ${RLIBS_PATH}/DiagrammeR/htmlwidgets/lib/mermaid/dist/mermaid.css

    # Download a more recent mermaid.js
    export MERMAID_VER=10.2.4
    export PATH2JS=${RLIBS_PATH}/DiagrammeR/htmlwidgets/lib/mermaid/dist/mermaid.slim.min.js
    wget \
      "https://cdnjs.cloudflare.com/ajax/libs/mermaid/${MERMAID_VER}/mermaid.min.js" \
      -O $PATH2JS

    # Delete default settings
    touch ${RLIBS_PATH}/DiagrammeR/htmlwidgets/lib/mermaid/dist/mermaid.css

    # Set 'securityLevel' to 'loose' to enable the use of HTML tags within node labels
    sed -i "" "s/securityLevel:\"strict\"/securityLevel:\"loose\"/g" $PATH2JS
    ```

## Render current presentation

1. Clone the project.

    ```bash
    git clone <URL>
    ```

2. Render the `.Rmd` file.

    ```bash
    export PJNAME='how-to-become-data-scientist'
    export mycmd=rmarkdown::render(\"${PJPATH}/${PJNAME}.Rmd\")
    Rscript -e $mycmd
    ```
