name: Generate Snake

on:
  schedule:
    - cron: "0 */12 * * *"
  workflow_dispatch:
  push:
    branches:
      - main

permissions:
  contents: write

jobs:
  generate:
    runs-on: ubuntu-latest
    timeout-minutes: 20
    steps:
      - name: Generate snake SVG
        uses: Platane/snk/svg-only@v3
        with:
          github_user_name: kunjbhuva7
          outputs: |
            dist/github-snake.svg
            dist/github-snake-dark.svg?palette=github-dark

      - name: Generate Pac-Man SVG
        uses: abozanona/pacman-contribution-graph@main
        with:
          github_user_name: kunjbhuva7
          github_token: ${{ secrets.GITHUB_TOKEN }}

      - name: Push to output branch
        uses: crazy-max/ghaction-github-pages@v4
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
