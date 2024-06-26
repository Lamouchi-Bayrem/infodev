FROM julia:1.10.0
LABEL author="A. Mhamdi <a_mhamdi@outlook.com>"

RUN apt-get update && apt-get install -y libgl1-mesa-dev

# Expose ports
EXPOSE 1234 2468 8000 9384

# Set ENV variables
ENV USERNAME mhamdi 
ENV USER_HOME_DIR /home/${USERNAME}
ENV JULIA_DEPOT_PATH ${USER_HOME_DIR}/.julia
ENV JL ${JULIA_DEPOT_PATH}/conda/3/bin/jupyter\ lab
ARG WORKING_DIR=${USER_HOME_DIR}/work

# Add user and set ownership
RUN useradd -ms /bin/bash  -p $(echo ${USERNAME} | openssl passwd -1 -stdin) ${USERNAME}

# Change user and working directory
USER ${USERNAME}
WORKDIR ${USER_HOME_DIR}
RUN mkdir -p ${WORKING_DIR}
WORKDIR ${WORKING_DIR}

# Copy TOML files (deps, ver, pkg, UUID)
COPY ./Codes/Project.toml ./

# Activate environment and add IJulia kernel, Pluto and JupyterLab IDE
RUN julia -e 'import Pkg; Pkg.activate("."); Pkg.instantiate(); Pkg.precompile()'
RUN julia -e 'import Pkg; Pkg.add(["Conda", "IJulia", "Pluto"]); Pkg.precompile()'
RUN julia -e 'import Conda; Conda.add("JupyterLab")'

# Default command
CMD julia
