# Random one-liners and snippets

## Copy some text files via your paste buffer

    clear; for i in *; do echo "cat << \"EOF\" > $i"; cat $i; echo "EOF"; done

## Add some swap, install docker tools

    cat << "EOF" > install_docker.sh
    if [ ! -f /swap ]; then
       SWAP_SIZE=$((2 * $(free |grep Mem | awk '{print $2 }')))
       dd if=/dev/zero of=/swap bs=1024 count=$SWAP_SIZE
       chmod 600 /swap
       echo "/swap swap swap defaults 0 0" >> /etc/fstab
       mkswap /swap
       swapon /swap
    fi

    apt-get update
    apt-get install -y \
       apt-transport-https \
       ca-certificates \
       curl \
       gnupg-agent \
       software-properties-common
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    apt-key fingerprint 0EBFCD88
    add-apt-repository    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
       $(lsb_release -cs) \
       stable"
    apt-get install -y docker-ce
    EOF
