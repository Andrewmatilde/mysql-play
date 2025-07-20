# mysql-play

1.1 业务场景

负载特征：纯写入负载，高并发写入
存储位置：云盘，挂载在 /data 目录

CREATE TABLE IF NOT EXISTS time_series_data (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    timestamp DATETIME(3) NOT NULL,
    device_id VARCHAR(100) NOT NULL,
    metric_name VARCHAR(50) NOT NULL,
    value DOUBLE NOT NULL,
    priority TINYINT NOT NULL DEFAULT 2,
    data TEXT COMMENT '随机负载数据，用于增大传输量',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    INDEX idx_timestamp (timestamp),
    INDEX idx_device_metric (device_id, metric_name),
    INDEX idx_priority (priority)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

需要 安装 mysql 并且 存储在 /data 目录 
并且 写好压力测试脚本 测试 标准 engine 下 写入 QPS
需要包括 写入 QPS 延迟 和最后的写入数据总存储大小
然后切换存储 engine 到 lsmtree 测试同样脚本下的 QPS

注意 data 字段只有 64 个字符，并且是固定 64 个字符