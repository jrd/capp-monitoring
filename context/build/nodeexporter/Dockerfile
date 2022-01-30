FROM prom/node-exporter:v1.3.1
CMD [ \
    "--path.procfs=/host/proc", \
    "--path.rootfs=/rootfs", \
    "--path.sysfs=/host/sys", \
    "--collector.filesystem.ignored-mount-points=^/(sys|proc|dev|host|etc)($$|/)" ]
