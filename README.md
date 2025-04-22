## gap-docker

[![docker-build](https://github.com/limakzi/gap-docker/actions/workflows/main.yaml/badge.svg)](https://github.com/limakzi/gap-docker/actions/workflows/main.yaml)

## Image types

```mermaid
    graph LR
        A[gap-docker] --> O1
        A[gap-docker] --> O2
    
        subgraph  O1 [ ]
            direction LR
            B[4.13.1] --> D[bare]
            B --> E[slim]
            B --> F[full]
        
            D --> H[gap-docker:4.13.1-bare]
            F --> K[gap-docker:4.13.1-full]
            E --> I[gap-docker:4.13.1-slim]
        end
        
        subgraph O2 [ ]
            direction LR
            B1[4.13.0] --> D1[bare]
            B1 --> E1[full]
            B1 --> F1[slim]
        
            D1 --> H1[gap-docker:4.13.0-bare]
            F1 --> K1[gap-docker:4.13.0-slim]
            E1 --> I1[gap-docker:4.13.0-full]
        end
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