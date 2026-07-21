ARG NEKO_VERSION=3.1.4
FROM ghcr.io/m1k1o/neko/google-chrome:${NEKO_VERSION}

# Neko disables DevTools through a managed policy. Browser Harness needs CDP,
# but the endpoint remains private and is published only on the host loopback.
RUN apt-get update \
  && apt-get install -y --no-install-recommends socat \
  && sed -i 's/"DeveloperToolsAvailability": 2/"DeveloperToolsAvailability": 0/' /etc/opt/chrome/policies/managed/policies.json \
  && grep -q '"DeveloperToolsAvailability": 0' /etc/opt/chrome/policies/managed/policies.json \
  && install -d -o neko -g neko /home/neko/.config/google-chrome-browser-harness \
  && rm -rf /var/lib/apt/lists/*

COPY --chown=neko:neko ["local-state.json", "/home/neko/.config/google-chrome-browser-harness/Local State"]
COPY supervisord.google-chrome.conf /etc/neko/supervisord/google-chrome.conf

# The branded web client is built from ../../client before deployment. Keeping
# it as a final layer avoids rebuilding the browser runtime for UI-only changes.
COPY client-dist/ /var/www/
