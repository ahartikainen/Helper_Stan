# Helper_Stan
Helper functions for CmdStan - works on both linux and osx


```
real_path() {
    [[ $1 = /* ]] && echo "$1" || echo "$PWD/${1#./}"
}

############ FILL HERE ############
stan_path="<full_path_here>/cmdstan-2.16.0"

stan_make() {
    local output_relative=${@: -1}
    local head_args=${@:1:$#-1} 
    local output=$(real_path $output_relative)
    echo "make -C $stan_path $head_args $output"
    make -C $stan_path $head_args $output
}

stan_summary() {
    local summary="bin/stansummary"
    local path=$stan_path/$summary
    if [ ! -f $path ]; then
        echo "Summary program is not compiled. Compiling now..."
        stan_cmd build
    fi
    $path $@
}

stan_stanc() {
    local stanc="bin/stanc"
    local path=$stan_path/$stanc
    if [ ! -f $path ]; then
        echo "Stanc program is not compiled. Compiling now..."
        stan_cmd build
    fi
    $path $@
}

stan_cmd() {
    echo "make -C $stan_path $@"
    make -C $stan_path $@
}

stan_sampling() {
     local filename=$1
     local args=${@:2}
     ./$filename sample data file=$filename.data.R args
}

stan_sampling_nchain() {
     local filename=$1
     local chains=$2
     local args=${@:3}
     for i in {1..4}
     do
        ./$filename sample id=$i data file=$filename.data.R $args output file=samples_$filename_$i.csv &
     done
}
```

# Usage

```
bash>: stan_cmd build -j4

# stanfile: ./bernoulli.stan
bash>: stan_make ./bernoulli
```
