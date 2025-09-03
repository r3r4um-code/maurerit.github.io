---
title: "NixOS as My New Home?"
date: 2025-09-13T00:00:00-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

I don't know why I never included it in my distrohopping months ago but NixOS was not on the list that I tried.  I really meant to try it out once but alas I just never did.  So last week I rectified that and installed NixOS on my spare harddrive.  The installation was just like any other distro install - select timezone, locale, disk to use, desktop environment to install, etc.  I don't recall the install taking very long either but most distro's don't take much time anymore with modern hardware.

Once I made it into Nix there was nothing new about what I saw.  Standard SDDM look and feel with a standard KDE Plasma default look and feel once I was in.  Where I started to notice the difference is when I inspected my path.  It looks like this: `/run/wrappers/bin /home/maurerit/.nix-profile/bin /nix/profile/bin /home/maurerit/.local/state/nix/profile/bin /etc/profiles/per-user/user/bin /nix/var/nix/profiles/default/bin /run/current-system/sw/bin`.  That kind of threw me for a loop.  I knew nix was different but I figured they did things to unbreak the FHS so that standard linux knowledge isn't ALL thrown out the window, lol.  Nope, the FHS is completely broken rendering NixOS a thing all it's own that also happens to run Linux, GNU Core utils and FOSS tools.

I have a basic understanding of NixOS after having talked quite a bit with a buddy that used to run it and loved it so I knew generally where to go.  The manual desktop entry in the application menu is always a welcome addition as well so I cracked that open and navigated to /etc/nixos and started editing the configuration.nix.  This file is surprisingly short for something that defines an entire system.  There's quite a bit of magic going on though as all you need to do to install KDE Plasma is to toggle one little property.  Not much different than installing a meta package on Arch or Debian or any other distribution though I guess but where it differs is that this isn't just lost in some bash history somewhere or completely hidden by the installer.  It's all in configuration.nix and you can easily see what is in your system.

Here is my current config:
```nix
# Edit this configuration file to define what should be installed on
# your system.  Help is available in the configuration.nix(5) man page
# and in the NixOS manual (accessible by running ‘nixos-help’).

{ config, pkgs, lib, ... }:
let
  home-manager = builtins.fetchTarball https://github.com/nix-community/home-manager/archive/release-25.05.tar.gz;
in
{
  imports =
    [ # Include the results of the hardware scan.
      ./hardware-configuration.nix
      (import "${home-manager}/nixos")
    ];

  # Bootloader.
  boot.loader.systemd-boot.enable = true;
  boot.loader.efi.canTouchEfiVariables = true;

  # Use latest kernel.
  boot.kernelPackages = pkgs.linuxPackages_latest;

  networking.hostName = "maurer-nixos-lappy"; # Define your hostname.
  # networking.wireless.enable = true;  # Enables wireless support via wpa_supplicant.

  # Configure network proxy if necessary
  # networking.proxy.default = "http://user:password@proxy:port/";
  # networking.proxy.noProxy = "127.0.0.1,localhost,internal.domain";

  # Enable networking
  networking.networkmanager.enable = true;

  # Set your time zone.
  time.timeZone = "America/New_York";

  # Select internationalisation properties.
  i18n.defaultLocale = "en_US.UTF-8";

  i18n.extraLocaleSettings = {
    LC_ADDRESS = "en_US.UTF-8";
    LC_IDENTIFICATION = "en_US.UTF-8";
    LC_MEASUREMENT = "en_US.UTF-8";
    LC_MONETARY = "en_US.UTF-8";
    LC_NAME = "en_US.UTF-8";
    LC_NUMERIC = "en_US.UTF-8";
    LC_PAPER = "en_US.UTF-8";
    LC_TELEPHONE = "en_US.UTF-8";
    LC_TIME = "en_US.UTF-8";
  };

  # Enable the X11 windowing system.
  # You can disable this if you're only using the Wayland session.
  services.xserver.enable = true;

  # Enable the KDE Plasma Desktop Environment.
  services.displayManager.sddm.enable = true;
  services.desktopManager.plasma6.enable = true;

  # Configure keymap in X11
  services.xserver.xkb = {
    layout = "us";
    variant = "";
  };

  # Enable CUPS to print documents.
  services.printing.enable = true;

  # Enable sound with pipewire.
  services.pulseaudio.enable = false;
  security.rtkit.enable = true;
  services.pipewire = {
    enable = true;
    alsa.enable = true;
    alsa.support32Bit = true;
    pulse.enable = true;
    # If you want to use JACK applications, uncomment this
    #jack.enable = true;

    # use the example session manager (no others are packaged yet so this is enabled by default,
    # no need to redefine it in your config for now)
    #media-session.enable = true;
  };

  # Enable touchpad support (enabled default in most desktopManager).
  # services.xserver.libinput.enable = true;

  # Define a user account. Don't forget to set a password with ‘passwd’.
  users.users.maurerit = {
    isNormalUser = true;
    description = "Matthew Maurer";
    extraGroups = [ "networkmanager" "wheel" ];
    uid = 1000;
    shell = pkgs.fish;
  };

  home-manager.users.maurerit = { config, pkgs, ... }: {
    home.packages = with pkgs; [
      kdePackages.kate
      fishPlugins.pure
    ];

    programs.git = {
      enable = true;
      userEmail = "maurer.it@gmail.com";
      userName = "Matthew Maurer";
    };

    home.file.".config/kwinrc".source = ./dotfiles/kwinrc;

    home.file.".config/powerdevilrc".source = ./dotfiles/powerdevilrc;

    home.file.".config/kcminputrc".source = ./dotfiles/kcminputrc;

    home.file.".config/kscreenlockerrc".source = ./dotfiles/kscreenlockerrc;

    home.file.".config/plasma-org.kde.plasma.desktop-appletsrc".source = ./dotfiles/plasma-org.kde.plasma.desktop-appletsrc;

    home.file.".config/plasmashellrc".source = ./dotfiles/plasmashellrc;

    home.stateVersion = "25.05";
  };

  # Install firefox.
  programs.firefox.enable = true;
  programs.steam.enable = true;
  programs.fish.enable = true;
  programs.git.enable = true;

  # Allow unfree packages
  nixpkgs.config.allowUnfree = true;

  # List packages installed in system profile. To search, run:
  # $ nix search wget
  environment.systemPackages = with pkgs; [
    discord
    signal-desktop
    protonup-qt
    brave
    fastfetch
    fish
    _1password-gui

    #Development dependencies
    h2
    jetbrains.idea-ultimate
    jetbrains.webstorm
    jetbrains.datagrip
    jetbrains.goland
    jdk24

    #System needs
    nfs-utils
  ];

  # Some programs need SUID wrappers, can be configured further or are
  # started in user sessions.
  # programs.mtr.enable = true;
  # programs.gnupg.agent = {
  #   enable = true;
  #   enableSSHSupport = true;
  # };

  # List services that you want to enable:

  # Enable the OpenSSH daemon.
  # services.openssh.enable = true;

  # Open ports in the firewall.
  # networking.firewall.allowedTCPPorts = [ ... ];
  # networking.firewall.allowedUDPPorts = [ ... ];
  # Or disable the firewall altogether.
  networking.firewall.enable = false;

  # This value determines the NixOS release from which the default
  # settings for stateful data, like file locations and database versions
  # on your system were taken. It‘s perfectly fine and recommended to leave
  # this value at the release version of the first install of this system.
  # Before changing this value read the documentation for this option
  # (e.g. man configuration.nix or on https://nixos.org/nixos/options.html).
  system.stateVersion = "25.05"; # Did you read the comment?

}
```

One of the key segments is the part that starts with `home-manager.users.maurerit` as this is a module that is intended to configure the home folders of users.  This module is called [Home Manager](https://github.com/nix-community/home-manager) and I'm currently using it to put some config files in place for KDE, update my git config and setup the pure fish config inside my home folder.  I am still messing around with this module as people talk about it heavily and I feel like I'm just scratching the surface with my usage of it.  One thing I want home manager to manage for me is the shell.  I _think_ it has options for fish specifically so I want to get in there and get fish to display fastfetch when starting up.  Other than that I'm not sure what else I need to do to be happy with the install.

Speaking of being happy in the install, I was up and running with that config in just under a couple of hours.  The documentation is really good, it leaves you hints in the config file via comments and generally the experience is really cool.  I was quite happy in the OS surfing and using it like I normally do.  I haven't fully setup my development environment with a project and coded on the platform but that is coming.  I don't have plans to distro hop much more on the ASUS laptop as I do want to give nix a chance so I might actually start using it completely just to see if it's for me.  I think what's holding me back is that I'm not super excited about it.  Sure it was cool and it has some cool features but it doesn't exactly get me excited to use it.  So while I'd be happy to use it if it were my only option, I am really thinking that Arch is for me and specifically CachyOS.

I'll keep these pages posted with my nix adventures.