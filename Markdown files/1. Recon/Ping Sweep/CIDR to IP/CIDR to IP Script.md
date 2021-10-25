# CIDR to IP Script

## SYNOPSIS
`./cidr-to-ip.sh [OPTION(only one)] [STRING/FILENAME] > Output.txt`

## DESCRIPTION
-h Displays this help screen
-f Forces a check for network boundary when given a STRING(s)
-i Will read from an Input file (file should contain one CIDR per line) (no network boundary check)
-b Will do the same as –i but with network boundary check
 
## EXAMPLES
./cidr-to-ip.sh 192.168.0.1/24
./cidr-to-ip.sh 192.168.0.1/24 10.10.0.0/28
./cidr-to-ip.sh -f 192.168.0.0/16
./cidr-to-ip.sh -i inputfile.txt
./cidr-to-ip.sh -b inputfile.txt
```
#!/bin/bash
############################
##  Methods
############################   
prefix_to_bit_netmask() {
    prefix=$1;
    shift=$(( 32 - prefix ));
    bitmask=""
    for (( i=0; i < 32; i++ )); do
        num=0
        if [ $i -lt $prefix ]; then
            num=1
        fi
        space=
        if [ $(( i % 8 )) -eq 0 ]; then
            space=" ";
        fi
        bitmask="${bitmask}${space}${num}"
    done
    echo $bitmask
}
bit_netmask_to_wildcard_netmask() {
    bitmask=$1;
    wildcard_mask=
    for octet in $bitmask; do
        wildcard_mask="${wildcard_mask} $(( 255 - 2#$octet ))"
    done
    echo $wildcard_mask;
}
check_net_boundary() {
    net=$1;
    wildcard_mask=$2;
    is_correct=1;
    for (( i = 1; i <= 4; i++ )); do
        net_octet=$(echo $net | cut -d '.' -f $i)
        mask_octet=$(echo $wildcard_mask | cut -d ' ' -f $i)
        if [ $mask_octet -gt 0 ]; then
            if [ $(( $net_octet&$mask_octet )) -ne 0 ]; then
                is_correct=0;
            fi
        fi
    done
    echo $is_correct;
}
#######################
##  MAIN
#######################
OPTIND=1;
getopts "fibh" force;
shift $((OPTIND-1))
if [ $force = 'h' ]; then
    echo ""
    echo -e "THIS SCRIPT WILL EXPAND A CIDR ADDRESS.\n\nSYNOPSIS\n  ./cidr-to-ip.sh [OPTION(only one)] [STRING/FILENAME]\nDESCRIPTION\n -h  Displays this help screen\n -f  Forces a check for network boundary when given a STRING(s)\n    -i  Will read from an Input file (no network boundary check)\n  -b  Will do the same as –i but with network boundary check\n\nEXAMPLES\n    ./cidr-to-ip.sh  192.168.0.1/24\n   ./cidr-to-ip.sh  192.168.0.1/24 10.10.0.0/28\n  ./cidr-to-ip.sh  -f 192.168.0.0/16\n    ./cidr-to-ip.sh  -i inputfile.txt\n ./cidr-to-ip.sh  -b inputfile.txt\n"
    exit
fi
if [ $force = 'i' ] || [ $force = 'b' ]; then
    old_IPS=$IPS
    IPS=$'\n'
    lines=($(cat $1)) # array
    IPS=$old_IPS
        else
            lines=$@
fi
for ip in ${lines[@]}; do
    net=$(echo $ip | cut -d '/' -f 1);
    prefix=$(echo $ip | cut -d '/' -f 2);
    do_processing=1;
    bit_netmask=$(prefix_to_bit_netmask $prefix);
    wildcard_mask=$(bit_netmask_to_wildcard_netmask "$bit_netmask");
    is_net_boundary=$(check_net_boundary $net "$wildcard_mask");
    if [ $force = 'f' ] && [ $is_net_boundary -ne 1 ] || [ $force = 'b' ] && [ $is_net_boundary -ne 1 ] ; then
        read -p "Not a network boundary! Continue anyway (y/N)? " -n 1 -r
        echo    ## move to a new line
        if [[ $REPLY =~ ^[Yy]$ ]]; then
            do_processing=1;
        else
            do_processing=0;
        fi
    fi
    if [ $do_processing -eq 1 ]; then
        str=
        for (( i = 1; i <= 4; i++ )); do
            range=$(echo $net | cut -d '.' -f $i)
            mask_octet=$(echo $wildcard_mask | cut -d ' ' -f $i)
            if [ $mask_octet -gt 0 ]; then
                range="{$range..$(( $range | $mask_octet ))}";
            fi
            str="${str} $range"
        done
        ips=$(echo $str | sed "s, ,\\.,g"); ## replace spaces with periods, a join...
        eval echo $ips | tr ' ' '\n'
else
exit
    fi

done
```