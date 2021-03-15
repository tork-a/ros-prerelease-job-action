ros-prerelease-job-action
=========================

[![GitHub Action Status](https://github.com/tork-a/ros-prerelease-job-action/actions/workflows/main.yml/badge.svg)](https://github.com/tork-a/ros-prerelease-job-action/actions/workflows/main.yml)


GitHub Action for running ROS prerelease test using devel jobs.

It builds the code and runs the tests to check for regressions.
It operates on a single source repository and is triggered for every commit to a specific branch.
See https://github.com/ros-infrastructure/ros_buildfarm/blob/master/doc/jobs/devel_jobs.rst for mor info


You could enable GitHub Action by adding following file in `.github/workflows/ci.yml`

```
on: [push]

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      fail-fast: false
      matrix:
        include:
          - build: kinetic_amd64
            ROS_DISTRO_NAME : kinetic
          - build: melodic_amd64
            ROS_DISTRO_NAME : melodic

    name: ${{ matrix.build }}

    steps:
      - name: Chcekout
        uses: actions/checkout@v2
        with:
          fetch-depth: 2
      - name: Run prerelease test
        uses: tork-a/ros-prerelease-job-action@main
        with:
          ros_distro_name: ${{matrix.ROS_DISTRO_NAME}}
```

