# Helper_Stan
Helper functions for cmdstan - works on both linux and osx


```
real_path() {
    [[ $1 = /* ]] && echo "$1" || echo "$PWD/${1#./}"
}

stan_path="<full_path_here>/cmdstan-2.16.0"

stan_make() {
    local output_relative=${@: -1}
    local head_args=${@:1:$#-1} 
    local output=$(real_path $output_relative)
    make -C $stan_path $head_args $output
}

stan_summary() {
    local summary="bin/stansummary"
    local path=$stan_path/$summary
    $path $@
}

stan_stanc() {
    local stanc="bin/stanc"
    local path=$stan_path/$stanc
    $path $@
}

stan_cmd() {
    make -C $stan_path $@
}
```
