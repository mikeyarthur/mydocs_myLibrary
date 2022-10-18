

<span style="font-size: 60px; background-color: rgb(255, 255, 255);">
	<span style="color: rgb(0, 0, 0);">事件线程</span>
	<span style="color: rgb(0, 0, 0); background-color: rgb(255, 153, 0);">重构</span>
</span>



---
## 目录页
---
[TOC]


---
## TODO
---

- [x] 事件线程框架搭建
- [x] 事件线程组件填充
- [x] 事件线程整机调试
- [ ] 中断后数据的初始化挪到wait_interrupt之前
- [ ] 厘清image_usage用法
- [ ] 厘清scan_slot_idx用法
- [ ] 厘清b_leaving_too_fast_detected用法
- [ ] 算法回归
- [ ] 项目自定义流程
- [x] 事件线程说明
	- [x] 框架说明
	- [x] 框架demo（采图子流程为例）
	- [x] goto语句的转化
	- [x] continue语句的转化
- [x] TODO项目清除
- [x] FIXME项目清除
- [x] controlState说明
- [x] err全局变量转局部变量



---
## 1. 事件线程框架
---

```C++

g_context->b_device_event_thread_running = true;
while (g_context->b_device_event_thread_running) {
    // use default controlState, case normal
    // update controlState, case captureRetry or whatever
    __et_control_state(NULL);

    /* 1.0. getReadyHandleEventThreadState */
    do {
        /* 1.1. waitForReadySubFlow */
        if (g_etFlow->eventReady.preHandler) {
            g_etFlow->eventReady.preHandler(NULL);
        }
        if (!g_etFlow->eventReady.shouldDo) {
            FF_LOGI(ET_FM_LOG_TAG, "eventReady", "preHandler break");
            break;
        }
        if (g_etFlow->eventReady.handler) {
            g_etFlow->eventReady.handler(NULL);
        }
        if (g_etFlow->eventReady.postHandler) {
            g_etFlow->eventReady.postHandler(NULL);
        }
    } while(g_etFlow->eventReady.shouldDo);

    do {
        if (!g_etFlow->interruptReady.shouldDo) {
            FF_LOGI(ET_FM_LOG_TAG, "interruptReady", "shouldDo = false");
            break;
        }
        /* 1.2. getReadyPreHandleSubFlow */
        if (g_etFlow->interruptReady.preHandler) {
            g_etFlow->interruptReady.preHandler(NULL);
        }
             if (!g_etFlow->interruptReady.shouldDo) {
             FF_LOGI(ET_FM_LOG_TAG, "interruptReady", "preHandler break");
             break;
         }

         /* 1.3. waitInterruptSubFlow */
         if (g_etFlow->interruptReady.handler) {
             g_etFlow->interruptReady.handler(NULL);
         }

     } while(0);

     /* 1.4. getReadyPostHandleSubFlow */
     if (g_etFlow->interruptReady.postHandler) {
         if (g_etFlow->flag.shouldContinue) {
             continue;
         }
         g_etFlow->interruptReady.postHandler(NULL);
     }

     /* 2.0. interruptHandleEventThreadState */
     do {
         if (!g_etFlow->interrupt.shouldDo) {
             FF_LOGI(ET_FM_LOG_TAG, "interrupt", "shouldDo = false");
             break;
         }
         /* 2.1. interruptPreHandleSubFlow */
         if (g_etFlow->interrupt.preHandler) {
             g_etFlow->interrupt.preHandler(NULL);
         }
         if (!g_etFlow->interrupt.shouldDo) {
             FF_LOGI(ET_FM_LOG_TAG, "interrupt", "preHandler break");
             break;
         }

         /* 2.2. interruptSubFlow */
         if (g_etFlow->interrupt.handler) {
             g_etFlow->interrupt.handler(NULL);
         }

     } while(0);

     /* 2.3. interruptPostHandleSubFlow */
     if (g_etFlow->interrupt.postHandler) {
         if (g_etFlow->flag.shouldContinue) {
             continue;
         }
         g_etFlow->interrupt.postHandler(NULL);
     }


     /* 3.0. captureRetryHandleEventThreadState */
    do {
        if (!g_etFlow->captureRetry.shouldDo) {
            FF_LOGI(ET_FM_LOG_TAG, "captureRetry", "shouldDo = false");
            break;
        }
        /* 3.1. captureRetryPreHandleSubFlow */
        if (g_etFlow->captureRetry.preHandler) {
            g_etFlow->captureRetry.preHandler(NULL);
        }
        if (!g_etFlow->captureRetry.shouldDo) {
            FF_LOGI(ET_FM_LOG_TAG, "captureRetry", "preHandler break");
            break;
        }

        /* 3.2. captureRetrySubFlow */
        if (g_etFlow->captureRetry.handler) {
            g_etFlow->captureRetry.handler(NULL);
        }

    } while(0);

    /* 3.3. captureRetryPostHandleSubFlow */
    if (g_etFlow->captureRetry.postHandler) {
        if (g_etFlow->flag.shouldContinue) {
            continue;
        }
        g_etFlow->captureRetry.postHandler(NULL);
    }


    /* 4.0. captureHandleEventThreadState */
    do {
        if (!g_etFlow->capture.shouldDo) {
            FF_LOGI(ET_FM_LOG_TAG, "capture", "shouldDo = false");
            break;
        }
        /* 4.1. capturePreHandleSubFlow */
        if (g_etFlow->capture.preHandler) {
            g_etFlow->capture.preHandler(NULL);
        }
        if (!g_etFlow->capture.shouldDo) {
            FF_LOGI(ET_FM_LOG_TAG, "capture", "preHandler break");
            break;
        }

        /* 4.2. captureSubFlow */
        if (g_etFlow->capture.handler) {
            g_etFlow->capture.handler(NULL);
        }

    } while(0);

    /* 4.3. capturePostHandleSubFlow */
    if (g_etFlow->capture.postHandler) {
        if (g_etFlow->flag.shouldContinue) {
            continue;
        }
        g_etFlow->capture.postHandler(NULL);
    }

    /* 5.0. breakInHandleEventThreadState */
    do {
        if (!g_etFlow->breakIn.shouldDo) {
            FF_LOGI(ET_FM_LOG_TAG, "breakIn", "shouldDo = false");
            break;
        }
        /* 5.1. breakInPreHandleSubFlow */
        if (g_etFlow->breakIn.preHandler) {
            g_etFlow->breakIn.preHandler(NULL);
        }
        if (!g_etFlow->breakIn.shouldDo) {
            FF_LOGI(ET_FM_LOG_TAG, "breakIn", "preHandler break");
            break;
        }

        /* 5.2. breakInHandleSubFlow */
        if (g_etFlow->breakIn.handler) {
            g_etFlow->breakIn.handler(NULL);
        }

    } while(0);

    /* 5.3. breakInPostHandleSubFlow */
    if (g_etFlow->breakIn.postHandler) {
        if (g_etFlow->flag.shouldContinue) {
            continue;
        }
        g_etFlow->breakIn.postHandler(NULL);
    }

    /* 6.0. mainProcessHandleEventThreadState */
    do {
        if (!g_etFlow->mainProcess.shouldDo) {
            FF_LOGI(ET_FM_LOG_TAG, "mainProcess", "shouldDo = false");
            break;
        }
        /* 6.1. mainProcessPreHandleSubFlow */
        if (g_etFlow->mainProcess.preHandler) {
            g_etFlow->mainProcess.preHandler(NULL);
        }
        if (!g_etFlow->mainProcess.shouldDo) {
            FF_LOGI(ET_FM_LOG_TAG, "mainProcess", "preHandler break");
            break;
        }

        /* 6.2. mainProcessHandleSubFlow */
        if (g_etFlow->mainProcess.handler) {
            g_etFlow->mainProcess.handler(NULL);
        }

    } while(0);

    /* 6.3. mainProcessPostHandleSubFlow */
    if (g_etFlow->mainProcess.postHandler) {
        if (g_etFlow->flag.shouldContinue) {
            continue;
        }
        g_etFlow->mainProcess.postHandler(NULL);
    }
}
```


---
## 2. 流程图说明
---
### 2.1. 代码实现说明

```C++
while(true)
	do {
		// step 1
		// step 2
		// step 3
		// step 4
		// 实现continue
		break;
		
		do {
			// step 1
			// step 2
			// step 3
			// step 4
			// 抛转外层 loop 实现 goto/continue
			break;
		} while(0);
	} while(0);
}
```

### 2.2. 流程图



![[ft_fp_event_thread_moudules_flow_chart.png]]


---
## 3. goto语句的转化
---
![[ft_fp_event_thread_goto_continue_controlState.png]]


---
## 4. continue语句的转化
---
见[[fp_event_thread#3 goto语句的转化]]  与 [[fp_event_thread#2 流程图说明]]

---
## 5. controlState的使用
---
见[[fp_event_thread#3 goto语句的转化]]  与 [[fp_event_thread#2 流程图说明]]


---
## 6. 框架demo（采图子流程为例）
---
参考[[fp_event_thread#2 1 流程图]] 与 [[fp_event_thread#2 2 流程图]]

```C++
static int __capture_handler(void *data) {
    UNUSED_VAR(data);

    g_etFlow->subFlow.captureHandlerFingerTouch.shouldDo = false;
    g_etFlow->subFlow.captureHandlerFingerLeave.shouldDo = false;

    switch (g_context->finger_status) {
    case FF_FINGER_STAT_TOUCHED:
        g_etFlow->subFlow.captureHandlerFingerTouch.shouldDo = true;
        break;
    case FF_FINGER_STAT_RELEASE:
        g_etFlow->subFlow.captureHandlerFingerLeave.shouldDo = true;
        break;
    default:
        break;
    }

    do {
        /* captureHandlerFingerTouch */
        do {
            if (!g_etFlow->subFlow.captureHandlerFingerTouch.shouldDo) {
                FF_LOGI(ET_FM_LOG_TAG, "subFlow.captureHandlerFingerTouch", "shouldDo = false");
                break;
            }
            /* captureHandlerFingerTouchPreHandler */
            if (g_etFlow->subFlow.captureHandlerFingerTouch.preHandler) {
                g_etFlow->subFlow.captureHandlerFingerTouch.preHandler(NULL);
            }
            if (!g_etFlow->subFlow.captureHandlerFingerTouch.shouldDo) {
                FF_LOGI(ET_FM_LOG_TAG, "subFlow.captureHandlerFingerTouch", "preHandler break");
                break;
            }

            /* captureHandlerFingerTouchHandler */
            if (g_etFlow->subFlow.captureHandlerFingerTouch.handler) {
                g_etFlow->subFlow.captureHandlerFingerTouch.handler(NULL);
            }
        } while(0);

        /* captureHandlerFingerTouchPostHandler */
        if (g_etFlow->subFlow.captureHandlerFingerTouch.handler) {
            if (g_etFlow->flag.shouldContinue) {
                /* throw continue to outer loop */
                break;
            }
            g_etFlow->subFlow.captureHandlerFingerTouch.postHandler(NULL);
        }

        /* captureHandlerFingerLeave */
        do {
            if (!g_etFlow->subFlow.captureHandlerFingerLeave.shouldDo) {
                FF_LOGI(ET_FM_LOG_TAG, "subFlow.captureHandlerFingerLeave", "shouldDo = false");
                break;
            }
            /* captureHandlerFingerLeavePreHandler */
            if (g_etFlow->subFlow.captureHandlerFingerLeave.preHandler) {
                g_etFlow->subFlow.captureHandlerFingerLeave.preHandler(NULL);
            }
            if (!g_etFlow->subFlow.captureHandlerFingerLeave.shouldDo) {
                FF_LOGI(ET_FM_LOG_TAG, "subFlow.captureHandlerFingerLeave", "preHandler break");
                break;
            }

            /* captureHandlerFingerLeaveHandler */
            if (g_etFlow->subFlow.captureHandlerFingerLeave.handler) {
                g_etFlow->subFlow.captureHandlerFingerLeave.handler(NULL);
            }
        } while(0);

        /* captureHandlerFingerLeavePostHandler */
        if (g_etFlow->subFlow.captureHandlerFingerLeave.handler) {
            if (g_etFlow->flag.shouldContinue) {
                /* throw continue to outer loop */
                break;
            }
            g_etFlow->subFlow.captureHandlerFingerLeave.postHandler(NULL);
        }
    } while(0);

    return 0;
}
```


---
## 7. checklist
---
见


---
## 8. 组件填充
---
### 8.1. 流程定义
```C++
typedef struct {
    bool shouldDo;
    int (*preHandler)(void *data);
    int (*handler)(void *data);
    int (*postHandler)(void *data);
    // TODO: make it a linked list in order to make it more flexible strategy
} flow_struct_t;
```

### 8.2. 结构体
```C++
typedef struct {
    flow_struct_t eventReady;
    flow_struct_t interruptReady;
    flow_struct_t interrupt;
    flow_struct_t captureRetry;
    flow_struct_t capture;
    flow_struct_t breakIn;
    flow_struct_t mainProcess;
    struct {
        /* 1. interrupt=flow, post=postHandler, finger_touch=case finger touch */
        //flow_struct_t interruptPostFingerTouch;
        //flow_struct_t interruptPostFingerLeave;
        flow_struct_t interruptFingerTouch;
        flow_struct_t interruptFingerLeave;
        flow_struct_t captureHandlerFingerTouch;
        flow_struct_t captureHandlerFingerLeave;
    } subFlow;
    struct {
        uint32_t shouldContinue     : 1;
        // TODO: status log
        uint32_t eventReady         : 1;
        uint32_t interruptReady     : 1;
        uint32_t captureRetrying    : 1;
    } flag;
    struct {
        uint32_t fast_screen_unlock_keycode;
        bool enable_fast_screen_unlock;
        uint32_t burst_acquisition_num;
        uint32_t burst_acquisition_mode;
        uint32_t max_rescan_times[2]; // {p_rescan_times, i_rescan_times}
        uint32_t max_auth_timing_elapsed;
        ff_image_acquisition_mode_t image_acquisition_mode;
        ff_scanning_params_t scanning_params;
    } config;
    struct {
        ff_event_status_t event_state;
        //ff_trustlet_event_context_t *ctx0;
        ff_trustlet_event_context_t *ctx1;
        ff_capture_context_t *capture_ctx;
        ff_event_status_t handling_event;
        bool b_finger_detected_by_first_interrupt;
        bool b_event_driven_mode;                       /* b_event_driven_mode, capture image driven mode, capacitive fingerprint chips = false, optical = true */
        uint64_t handling_hotzone_touch_token;
        bool b_scanned;
        bool b_leaving_too_fast_detected;
        uint32_t image_usage[MAX_SCAN_TIMES][BURST_ACQUISITON_MAX];
        uint32_t scan_slot_idx[4];                      /* {p_rescan_index, i_rescan_index, of_all_index, i_last_valid_index} */
        uint32_t min_cpufreq_scaling[3];
        uint32_t max_cpufreq_scaling[3];
        uint32_t scanning_time;
        uint32_t auth_timing_elapsed;
        CONTROL_STATE_T cs;
    } data;
} event_thread_flow_t;
static event_thread_flow_t __hal_event_thread_flow, *g_etFlow = &__hal_event_thread_flow;
```

### 8.3. 函数指针指向
```C++

    /* Allocate native event context. */
    uint8_t *native_event_context_data = new uint8_t[sizeof(ff_trustlet_event_context_t)];
    if (!native_event_context_data) {
        FF_LOGE("failed to create native event context object.");
        return NULL;
    }
    // __hal_event_thread_flow initialize
    memset(g_etFlow, 0, sizeof(__hal_event_thread_flow));
    g_etFlow->data.ctx1 = TYPE_OF(ff_trustlet_event_context_t, native_event_context_data);
    g_etFlow->data.capture_ctx = NULL;
    g_etFlow->data.handling_event = FF_EVENT_STAT_UNKNOWN;
    g_etFlow->data.scanning_time  = UINT32_MAX;
    memset(g_etFlow->data.scan_slot_idx, 0, sizeof(g_etFlow->data.scan_slot_idx));
    memset(g_etFlow->data.image_usage, 0, sizeof(g_etFlow->data.image_usage));
    g_etFlow->config.max_rescan_times[RESCAN_MAX_PREVIOUS] = 5;
    g_etFlow->config.max_rescan_times[RESCAN_MAX_IDENTIFY] = 2;

    /* TODO: initialize g_etFlow */
    g_etFlow->eventReady.preHandler     = __wait_event_ready_preHandler;
    g_etFlow->eventReady.handler        = __wait_event_ready_handler;

    g_etFlow->interruptReady.preHandler = __wait_interrupt_ready_preHandler;
    g_etFlow->interruptReady.handler    = __wait_interrupt_ready_handler;

    g_etFlow->interrupt.preHandler      = __interrupt_preHandler;
    g_etFlow->interrupt.handler         = __interrupt_handler;
    g_etFlow->interrupt.postHandler     = __interrupt_postHandler;
    g_etFlow->subFlow.interruptFingerTouch.preHandler     = __interrupt_finger_touch_preHandler;
    g_etFlow->subFlow.interruptFingerTouch.handler        = __interrupt_finger_touch_handler;
    g_etFlow->subFlow.interruptFingerTouch.postHandler    = __interrupt_finger_touch_postHandler;
    g_etFlow->subFlow.interruptFingerLeave.preHandler     = __interrupt_finger_leave_preHandler;
    g_etFlow->subFlow.interruptFingerLeave.handler        = __interrupt_finger_leave_handler;
    g_etFlow->subFlow.interruptFingerLeave.postHandler    = __interrupt_finger_leave_postHandler;

    g_etFlow->captureRetry.preHandler      = __capture_retry_preHandler;
    g_etFlow->captureRetry.handler         = __capture_retry_handler;
    g_etFlow->captureRetry.postHandler     = __capture_retry_postHandler;

    g_etFlow->capture.preHandler      = __capture_preHandler;
    g_etFlow->capture.handler         = __capture_handler;
    g_etFlow->capture.postHandler     = __capture_postHandler;
    g_etFlow->subFlow.captureHandlerFingerTouch.preHandler    = __capture_handler_finger_touch_preHandler;
    g_etFlow->subFlow.captureHandlerFingerTouch.handler       = __capture_handler_finger_touch_handler;
    g_etFlow->subFlow.captureHandlerFingerTouch.postHandler   = __capture_handler_finger_touch_postHandler;
    g_etFlow->subFlow.captureHandlerFingerLeave.preHandler    = __capture_handler_finger_leave_preHandler;
    g_etFlow->subFlow.captureHandlerFingerLeave.handler       = __capture_handler_finger_leave_handler;
    g_etFlow->subFlow.captureHandlerFingerLeave.postHandler   = __capture_handler_finger_leave_postHandler;

    g_etFlow->breakIn.preHandler      = __breakIn_preHandler;
    g_etFlow->breakIn.handler         = __breakIn_handler;
    g_etFlow->breakIn.postHandler     = __breakIn_postHandler;

    g_etFlow->mainProcess.preHandler      = __mainProcess_preHandler;
    g_etFlow->mainProcess.handler         = __mainProcess_handler;
    g_etFlow->mainProcess.postHandler     = __mainProcess_postHandler;

    g_context->b_device_event_thread_running = true;
    while (g_context->b_device_event_thread_running) {
```


---
## 9. 算法回归
---
TODO: 重写capture的实现（读取文件的方式），函数指针指向即可


---
## 10. 项目自定义流程
---
TODO: 重写项目定制流程的实现，函数指针指向即可
