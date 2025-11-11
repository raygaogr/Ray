---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠== You can decompress Drawing data with the command palette: 'Decompress current Excalidraw file'. For more info check in plugin settings under 'Saving'


# Excalidraw Data

## Text Elements
Video ^rNSBlYfH

Inference State ^aR90TuFK

images: ndarray 【输入视频构成的多维数组】
num_frames: int 【视频帧数】
offload_video_to_cpu: bool 【是否将视频数据存在cpu上】
offload_state_to_cpu: bool 【是否将inference state存在cpu上】
video_height: int 【视频帧高】
video_width: int 【视频帧宽】
device: torch.device 【计算设备】
storage_device: torch.device 【存储设备】
point_inputs_per_obj: dict 【存储每个对象在每帧的点击输入，
                                    结构: {obj_idx: {frame_idx: point_tensor}}】
mask_inputs_per_obj: dict 【存储每个对象在每帧的掩码输入
                                    结构: {obj_idx: {frame_idx: mask_tensor}}】
cached_features: dict 【缓存最近访问帧的视觉特征
                              缓存内容: {frame_idx: (image_tensor, backbone_features)}】
constants: dict 【内存位置编码在所有帧和对象间相同】
obj_id_to_idx: dict 【obj id 到 obj index 的映射表】
obj_idx_to_id: dict 【obj index 到 obj id 的映射表】
obj_ids: list 【全部的obj的id信息】
output_dict_per_obj: dict 【存储每个obj正式的追踪信息
                                    结构: "cond_frame_outputs": {
                                                frame_idx: {
                                                    "pred_masks": tensor,           # 预测的mask
                                                    "maskmem_features": tensor,     # 记忆特征
                                                    "maskmem_pos_enc": tensor,      # 位置编码
                                                    "obj_ptr": tensor,              # 对象指针
                                                    "object_score_logits": tensor   # 对象置信度
                                                }
                                            },
                                            "non_cond_frame_outputs": {
                                                # 类似结构，但用于非条件帧
                                            }】
temp_output_dict_per_obj: dict 【存储用户交互产生的临时结果】
frames_tracked_per_obj: dict 【记录每个对象在每帧的追踪元信息
                                        结构: frame_idx: {
                                                   "reverse": bool  # 是否反向追踪
                                               }】
 ^IQ19vWvQ

init_state: 初始化inference state，得到视频中所有的images，并生成第一帧的缓存信息 ^Uj8fYpr1

reset_state: 清空所有的points和mask prompt ^XlyQ1CKF

%%
## Drawing
```compressed-json
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
```
%%