## gap-docker

[![docker-build](https://github.com/limakzi/gap-docker/actions/workflows/main.yaml/badge.svg)](https://github.com/limakzi/gap-docker/actions/workflows/main.yaml)

## Image types

```mermaid
    graph TD
        A[GAP System Docker Images] --> B[4.13.1]
        A --> C[4.14.0]
    
        B --> D[Full Image]
        B --> E[Bare Image]
    
        C --> F[Full Image]
        C --> G[Bare Image]
    
        D --> H[ghcr.io/limakzi/gap-docker:4.13.1-full]
        E --> I[ghcr.io/limakzi/gap-docker:4.13.1-bare]
    
        F --> J[ghcr.io/limakzi/gap-docker:4.14.0-full]
        G --> K[ghcr.io/limakzi/gap-docker:4.14.0-bare]
```

* Run a container:

```
docker run -it ghcr.io/limakzi/gap-docker:4.13.1-full
```

```
docker run -it ghcr.io/limakzi/gap-docker:4.14.0-full
```

```
docker run -it ghcr.io/limakzi/gap-docker:4.13.1-bare
```

```
docker run -it ghcr.io/limakzi/gap-docker:4.14.0-bare
```


## Authors
Although it was completely rewritten, this repository consists of various ideas from many repositories
* Kamil Zabielski -- [@limakzi](https://github.com/limakzi)
* James D. Mitchell [[1](1)] -- [@james-d-mitchell](https://github.com/james-d-mitchell)
* Sam Tetrooy [[2](2)] -- [@stertooystertooy](https://github.com/stertooy)

[1]: https://github.com/james-d-mitchell/gap-docker-minimalhttps://github.com/james-d-mitchell/gap-docker-minimal
[2]: https://github.com/stertooy/gda-image/