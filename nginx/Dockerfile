FROM nginx:1.30.0-alpine-slim

RUN addgroup -S nginxuser && \
    adduser -S -s /sbin/nologin nginxuser -G nginxuser

COPY nginx.conf /etc/nginx/conf.d/default.conf

RUN chown -R nginxuser:nginxuser /var/cache/nginx && \
    chown -R nginxuser:nginxuser /var/log/nginx && \
    chown -R nginxuser:nginxuser /etc/nginx/conf.d && \
    touch /var/run/nginx.pid && \
    chown -R nginxuser:nginxuser /var/run/nginx.pid

USER nginxuser

EXPOSE 80

HEALTHCHECK --interval=30s --timeout=3s --start-period=40s --retries=3 \
    CMD curl -f http://localhost/ || exit 1
