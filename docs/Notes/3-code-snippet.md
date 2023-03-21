# PHP 代碼片段紀錄



> Module Online DB  => SQL

```
$module->submission_status = 'Published';
$module->module_status = 'Active';
$module->published_by = $_SESSION['USER']->id;
$module->published_at = date('Y-m-d H:i:s');


UPDATE module 
SET submission_status = 'Published', module_status = 'Active' , published_by = 17313 ,  published_at = '2022-12-12 12:12:12'
WHERE submission_status= 'Draft' AND deleted_at is null
```



```
UPDATE module 
SET submission_status = 'Published', module_status = 'Active' , published_by = 17313 ,  expired_at = NULL
WHERE submission_status= 'Expired' AND deleted_at is null
```



> PHP CI 框架 判断是否为staff

```
invisible: <?php echo !(($USER->myrole == 'staff'))  ?>
```



> HRM SQL 年假延遲更新

```
SELECT * FROM `hrm_staff_leave_period` WHERE special_brought_forward_expiry_date = '2022-12-31' AND leave_period = 21;


UPDATE hrm_staff_leave_period
SET special_brought_forward_expiry_date ='2023-06-30' 
WHERE special_brought_forward_expiry_date = '2022-12-31' AND leave_period = 21;


SELECT * FROM `hrm_staff_leave_period` WHERE special_brought_forward_expiry_date = '2023-06-30' AND leave_period = 21;
```





> DEBUG 三寶

```
ini_set('display_errors', 1);
ini_set('display_startup_errors', 1);
error_reporting(E_ALL);
```





> 常用SQL

```
UPDATE sis_student_gpa_warning
SET timelastmail = NULL 
WHERE semester = 58 and status = 'Probation';
```



```
ALTER TABLE `sls_student2application` ADD `kpi_remark` TEXT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci NULL DEFAULT NULL AFTER `booklet_at`, ADD `booklet_remark` TEXT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci NULL DEFAULT NULL AFTER `kpi_remark`;
```



```
UPDATE sls_student2application
SET booklet_remark = booklet_at
```



```
SELECT *, p.fullname as fullname, IF(p.level='MP',5,4) education ,p.name,p.chinese_programme_name
FROM hsmc_myhsmc.hsmc_programme p 
LEFT JOIN db_ams.ams_programme ap ON (p.fullname=ap.programme_code)
WHERE ap.id IS NULL 
AND fullname NOT IN ('BS','UNCLASSIFIED')
```

