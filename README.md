![Arch-Codespace](https://j.top4top.io/p_3539nzyyz1.png)

# Tutorial

This tutorial shows how to set up Arch Linux container in GitHub Codespaces.

---

## 1. Update & Upgrade System

```bash
sudo apt update && sudo apt upgrade -y
````

---

## 2. Create Project Directory (Optional)

```bash
mkdir -p ~/arch-codespace
cd ~/arch-codespace
```

---

## 3. Create Dockerfile

```bash
echo 'FROM archlinux:latest

RUN pacman -Syu --noconfirm && \
    pacman -S --noconfirm \
    git curl wget python python-pip base-devel sudo && \
    pacman -Scc --noconfirm

RUN useradd -ms /bin/bash rosemary && echo "rosemary:arch" | chpasswd && usermod -aG wheel rosemary
RUN echo "rosemary ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

USER rosemary
WORKDIR /home/rosemary

CMD ["/bin/bash"]' > Dockerfile
```

**Notes:** Root password for `rosemary` is `arch`.

---

## 4. Create docker-compose.yml

```bash
echo 'version: "3.8"

services:
  arch:
    build: .
    container_name: arch-cs
    tty: true
    stdin_open: true
    volumes:
      - arch-data:/home/

volumes:
  arch-data:' > docker-compose.yml
```

**Notes:** Volume `arch-data` stores persistent data.

---

## 5. Build Container

```bash
docker compose build
```

---

## 6. Run Container

```bash
docker compose up -d
```

---

## 7. Access Kali Container

```bash
docker exec -it arch-cs /bin/bash
```

---

## Notes

* Codespaces containers stop after idle.
* Save important files in volume (`arch-data`) or repository.
* Default user: `rosemary`, root password: `arch`.

---

## References

* [Arch Linux Docker Images](https://hub.docker.com/r/archlinux/archlinux)
* [GitHub Codespaces Documentation](https://docs.github.com/en/codespaces)
