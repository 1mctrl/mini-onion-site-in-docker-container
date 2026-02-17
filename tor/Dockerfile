FROM alpine

RUN apk add --no-cache tor

RUN mkdir -p /var/lib/tor && \
    chown -R tor:tor /var/lib/tor

COPY torrc /etc/tor/torrc

USER tor

CMD ["tor", "-f", "/etc/tor/torrc"]
