# Note: Ubuntu is being used due to familiarity only
FROM ubuntu:18.04

COPY . /app
WORKDIR /app

USER root

# Julia install dependencies and Python development
RUN apt-get update && apt-get install -yq --no-install-recommends \
    wget \
    ca-certificates \
    python3 \
    python3-dev \
    python3-pip


# Julia dependencies
# Install Julia packages in /opt/julia instead of $HOME
ENV JULIA_DEPOT_PATH=/opt/julia
ENV JULIA_PKGDIR=/opt/julia
ENV JULIA_VERSION=1.0.2

RUN mkdir /opt/julia-${JULIA_VERSION} && \
    cd /tmp && \
    wget -q https://julialang-s3.julialang.org/bin/linux/x64/`echo ${JULIA_VERSION} | cut -d. -f 1,2`/julia-${JULIA_VERSION}-linux-x86_64.tar.gz && \
    echo "e0e93949753cc4ac46d5f27d7ae213488b3fef5f8e766794df0058e1b3d2f142 *julia-${JULIA_VERSION}-linux-x86_64.tar.gz" | sha256sum -c - && \
    tar xzf julia-${JULIA_VERSION}-linux-x86_64.tar.gz -C /opt/julia-${JULIA_VERSION} --strip-components=1 && \
    rm /tmp/julia-${JULIA_VERSION}-linux-x86_64.tar.gz
RUN ln -fs /opt/julia-*/bin/julia /usr/local/bin/julia

# Install PyJulia requirement PyCall
RUN julia -e 'import Pkg; Pkg.update()' && \
    julia -e 'import Pkg; Pkg.add("PyCall")'

RUN julia -e 'import Pkg; Pkg.update; Pkg.add("Pandas")'
RUN julia -e 'import Pkg; Pkg.update; Pkg.add("IJulia")'
RUN julia -e 'import Pkg; Pkg.update; Pkg.add("DataFrames")'
RUN julia -e 'import Pkg; Pkg.update; Pkg.add("Distributed")'
RUN julia -e 'import Pkg; Pkg.update; Pkg.add("Dates")'
RUN julia -e 'import Pkg; Pkg.update; Pkg.add("CSV")'
RUN julia -e 'import Pkg; Pkg.update; Pkg.add("DataFramesMeta")'
RUN julia -e 'import Pkg; Pkg.update; Pkg.add("AcceleratedArrays")'
RUN julia -e 'import Pkg; Pkg.update; Pkg.add("BenchmarkTools")'
RUN julia -e 'import Pkg; Pkg.update; Pkg.add("ShiftedArrays")'

# Install PyJulia
RUN pip3 install julia
RUN pip3 install setuptools
RUN pip3 install pandas_redshift
RUN pip3 install cython
RUN pip3 install numba

RUN apt-get install -y jupyter
RUN apt-get insatll -y gcc

# Use the command python-jl, because finding the julia install is hard
CMD ["python-jl", "app.py"]

#jupyter notebook --ip 0.0.0.0 --port 8081 --no-browser --allow-root  #port 8081 or whatever set up with docker run command
#remember to use token for jupyter in browser

