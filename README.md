# Avian Blasters Project Work Report

This [repository](https://github.com/unibo-dtm-se-2425-AvianBlasters/report) contains the project report and documentation related to Avian Blasters (or Avian Blasters: The Avians Strike Back), a Python developed software, inspired by arcade games such as Space Invaders and Galaga, for a University Software Engineering project work.

- To access the project work documentation, click [here](https://unibo-dtm-se-2425-avianblasters.github.io/report/).

- To access the repository containing all of the project work discussed in the report, click [here](https://github.com/unibo-dtm-se-2425-AvianBlasters/artifact).

- To access the template used for this report, click [here](https://github.com/unibo-dtm-se/template-project-work).

## How to visualise a preview of the report, locally

You need to install __Ruby__ on your machine (instructions below).

1. Assuming that Ruby is correctly installed, you need to clone your report repository on your machine:
    
    ```bash
    git clone https://github.com/unibo-dtm-se-2425-AvianBlasters/report
    ```

2. Then, you need to restore Jeckyll's dependencies.
From within the root of your repository, run the following command:

    ```bash
    bundler install
    ```


3. Finally, you may run the following command from the root of your repository:

    ```bash
    bundler exec jekyll serve
    ```

    The output of that command should tell you the local URL where the preview of your site is available.
    Most commonly, it will be <http://127.0.0.1:4000>.

4. Open your browser, and navigate to the URL provided by the previous command
    + from now on, until you stop the `bundler exec jekyll serve` command, any change you make to the `.md` files will be automatically reflected in the preview
    + you may stop the preview by pressing `Ctrl+C` in the terminal

## How to install Ruby and Jekyll

Follow instructions from here: <https://jekyllrb.com/docs/installation/>
