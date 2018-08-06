# Shell script to install necessary pacakges
$install_script = <<INSTALL_SCRIPT
apk add --update python py-pip groff
pip install --upgrade pip
pip install awscli
INSTALL_SCRIPT

# Shell script to set the AWS env vars in the guest
# based on the same env vars set in the host
$env_vars_script = <<ENV_VARS_SCRIPT
tee "/home/vagrant/.bash_profile" > "/dev/null" <<EOF
# AWS environment variables.
export AWS_DEFAULT_REGION=#{ENV['AWS_DEFAULT_REGION']}
export AWS_ACCESS_KEY_ID=#{ENV['AWS_ACCESS_KEY_ID']}
export AWS_SECRET_ACCESS_KEY=#{ENV['AWS_SECRET_ACCESS_KEY']}
EOF
ENV_VARS_SCRIPT

# Use Vagrant configuration version "2"
Vagrant.configure("2") do |config|
	# Start with generic/alpine38 vagrant box
	config.vm.box = "generic/alpine38"

	# Note: Unlike default vagrant behavior, current working directory
	# will not be mounted to /vagrant in the guest. This is because
	# the box used here (generic/alpine38) does not come with the
	# virtualbox plugins to enable folder sharing between host and guest.
	#
	# This isn't too bad as for the purpose of this vagrant project
	# we do not need folder sharing.

	# Install the awscli
	config.vm.provision "shell", inline: $install_script

	# Set the AWS env vars
	config.vm.provision "shell", inline: $env_vars_script, run: "always"

	# Customize the VM (only on virtual box)
	config.vm.provider "virtualbox" do |v|
		v.name = "awscli-vm"
		v.linked_clone = true
		v.memory = 512
		v.cpus = 1
	end
end
