##数据库事务,锁,索引,慢查询概述

```
DROP PROCEDURE IF EXISTS create_report_index;

DELIMITER $$
CREATE PROCEDURE create_report_index()
BEGIN
    IF NOT EXISTS(
                SELECT * FROM information_schema.statistics
                WHERE TABLE_SCHEMA = 'deepflow'
                AND `table_name`='report'
                AND `index_name` = 'report_search_index'
            )
    THEN
    ALTER TABLE `report` ADD INDEX report_search_index (`begin_at`,`end_at`,`policy_id`);
    END IF;

    IF NOT EXISTS(
                SELECT * FROM information_schema.statistics
                WHERE TABLE_SCHEMA = 'deepflow'
                AND `table_name`='report'
                AND `index_name` = 'created_at'
            )
    THEN
    ALTER TABLE `report` ADD INDEX created_at (`created_at`);
    END IF;

    IF NOT EXISTS(
                SELECT * FROM information_schema.statistics
                WHERE TABLE_SCHEMA = 'deepflow'
                AND `table_name`='report_data'
                AND `index_name` = 'create_time'
            )
    THEN
    ALTER TABLE `report_data` ADD INDEX create_time (`create_time`);
    END IF;

END $$
DELIMITER ;

CALL create_report_index();

```
