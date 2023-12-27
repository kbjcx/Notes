# Introduction
<p style="text-indent:2em">锂离子电池具有能量比高、循环寿命长和自放电率低等特点，在电动汽车能源供应方面得到了广泛应用。但是随着锂离子电池的老化，锂离子电池内阻增加，导致电池电解液泄露、短路甚至是热失控等问题。能量管理系统能够通过核电状态（State of Charge，SOC）与健康状态（State of Health，SOH）来确保锂电池状态处于安全工作的范围内。因此有必要在锂电池充放电循环中对 SOC 和 SOH 进行准确的估计，为下一步指定运维计划提供依据。其中，SOC 是指一定放电倍率下当前剩余容量与额定容量的比值【1】，用来量化当前电池所剩余的能量；而 SOH 一般定义为衰减后的容量与标称容量的比值。【2,3】

<p style="text-indent:2em">目前针对 SOC 与 SOH 估计的研究较多，主流研究可以分为基于模型和数据驱动两大类。

<p style="text-indent:2em">基于模型的方法通常采用电池等效电路模型（Equivalent Circuit Model，ECM）或电化学模型（Electrochemical Model，EM）的方法。Yu 等人[12]提出了一种基于局部线性化的线性卡尔曼滤波（Linear Kalman Filter，LKF）方法用于锂离子电池的 SOC 估计。为了解决预测过程中的测量噪声和观测噪声问题，Xiong 等人[14]使用了自适应扩展卡尔曼滤波（Adaptive Extended Kalman Filter，AEKF）方法结合 OCV-SOC 表建立了在线 SOC 估计，有效避免了算法的发散和偏差。Tang 等人[47]为了应对非受控环境中的老化波动，提出了一种基于模型的梯度校正粒子滤波方法来预测锂离子电池的老化过程。Miao 等人[48]使用 UPF 来估计电池的容量衰减，结果误差在5%以内。

<p style="text-indent:2em">Tian 等人[29]针对 LiFePO4电池在 SOC 中间范围内的平坦开路电压特性，提出了一种基于深度神经网络（Deep Neural Network，DNN）的 SOC 估计方法。[赵赛超](https://www.sciencedirect.com/science/article/pii/S2352152X22009082)将广义学习系统（BLS）算法与长短期记忆神经网络（LSTM NN）相结合，建立了融合神经网络模型，对锂离子电池容量和 RUL 进行了出色的预测。Zhao 等人[63]为了减少计算资源，将宽度学习系统（Broad Learning System，BLS）和长短期记忆网络（LSTM）相结合，开发了一种融合神经网络模型。Ma 等人[61]首先利用皮尔逊相关系数选择了与电池容量相关性更高的特征，然后提出了一种利用差分进化灰狼优化器进行超参数选择的改进 LSTM 模型。

综上所述，锂离子电池的 SOC 与 SOH 估计方法已经比较成熟，但大多是对 SOC 与 SOH 单独进行估计，而忽略了 SOC 与 SOH 之间的联系。因此将 SOC 与 SOH 进行联合估计的研究是必要的。张欣等提出了一种基于 GWO-BP 神经网络的 SOH-SOC 联合估计模型，利用 SOH 来修正 Ah 积分方法。但是安时积分的准确度很大程度上受初始 SOC 的准确性以及累积误差的影响。[蒋波](https://www.sciencedirect.com/science/article/pii/S0306261919312930)等提出了一种基于自适应变量多时间尺度框架的电池 SOC 和容量联合估计方法，基于自适应扩展卡尔曼滤波器自适应更新噪声协方差来提高估计的准确性。[胡盼盼](https://www.mdpi.com/1996-1073/16/14/5313)等提出了一种基于非线性状态空间重构(NSSR)和长短期记忆(LSTM)神经网络的 SOC 和 SOH 联合估计方法。目前对 SOC 与 SOH 联合估计的研究还比较少，并且大多为基于模型的方法，

# Method