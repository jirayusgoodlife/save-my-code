```mermaid
graph TD
    LF_1[leave_history] -->
	    |lhis_status = 1 or 'Y'| LF_2[appv_flow_person]
    LF_2 --> LF_3{apfp_bill_type = 'leave'}
    LF_3 -->|apfp_status = 'Y'| LF_bt3[bot_type = 3]
    LF_3 -->|apfp_status = 'N'| LF_bt4[bot_type = 4]
    LF_3 -->|apfp_status = 'E'| LF_bt6[bot_type = 6]
```
