
GPU оптимизированными для виртуальной Машины размера: специализированные виртуальных машин с одного или нескольких графических процессоров NVIDIA. Эти размеры предназначены для рабочих нагрузок с большим объемом вычислений, графикой и визуализации. Этой статье содержатся сведения о количестве и тип графических процессоров, ЦП, диски с данными и сетевых адаптеров а также хранения пропускной способности и пропускную способность сети для каждого размера на группы. 

* **Контекст Именования, NCv2 и ND** размеры оптимизированы для вычислений и интенсивно сетевых приложений и алгоритмов, включая приложения на основе CUDA и OpenCL и моделирования, AI и углубленного обучения. 
* **NV** предназначены для удаленного визуализации, потоковой передачи, игры, кодировки и сценариев VDI, использование платформы, например OpenGL и DirectX и оптимизации размеров.  


## <a name="nc-instances"></a>Экземпляры NC

Контекст Именования экземпляров на платформе [NVIDIA Tesla K80](http://images.nvidia.com/content/pdf/kepler/Tesla-K80-BoardSpec-07317-001-v05.pdf) карты. Пользователи могут функционируют через данные быстрее за счет использования CUDA для просмотра приложений энергии, приводит к сбою моделирования, луча трассировка отрисовки, углубленного обучения и многое другое. Конфигурация NC24r обеспечивает низкой задержкой и высокой пропускной способностью сетевого интерфейса, оптимизированный для тесно связанные параллельных вычислений рабочих нагрузок.


| Размер | vCPU | Память, ГиБ | Временное хранилище (SSD): ГиБ | Графический процессор | Максимальное количество дисков данных |
| --- | --- | --- | --- | --- | --- |
| Standard_NC6 |6 |56 | 380 | 1 | 24 |
| Standard_NC12 |12 |112 | 680 | 2 | 48 |
| Standard_NC24 |24 |224 | 1440 | 4. | 64 |
| Standard_NC24r* |24 |224 | 1440 | 4. | 64 |

1 графический процессор — половина графической карты K80.

* С поддержкой RDMA

## <a name="ncv2-instances"></a>Экземпляры NCv2

Экземпляры NCv2 — это следующее поколение машин серии NC, на платформе [NVIDIA Tesla P100](http://images.nvidia.com/content/tesla/pdf/nvidia-tesla-p100-datasheet.pdf) GPU. Этих графических процессоров может предоставить больше, чем 2 x вычислительную производительность текущего контекста Именования ряда. Клиенты могут использовать эти обновленные GPU для традиционных рабочих нагрузок HPC моделирование резервуаров, ДНК последовательного выполнения, анализа Белок, Монте-Карло, и другие. Последовательность NC серии NCv2 предлагает конфигурации с низкой задержкой и высокой пропускной способностью сетевого интерфейса, оптимизированный для тесно связанные параллельных вычислений рабочих нагрузок.

> [!IMPORTANT]
> Для этого семейства размер квоты виртуальных процессоров (основной) в вашей подписке изначально установлено значение 0 в каждом регионе. [Запросить увеличение квоты виртуальных процессоров](../articles/azure-supportability/resource-manager-core-quotas-request.md) для этого семейства в [регион](https://azure.microsoft.com/regions/services/).
>

| Размер | vCPU | Память, ГиБ | Временное хранилище (SSD): ГиБ | Графический процессор | Максимальное количество дисков данных |
| --- | --- | --- | --- | --- | --- |
| Standard_NC6_v2 |6 |112 | 336 | 1 | 12 |
| Standard_NC12_v2 |12 |224 | 672 | 2 | 24 |
| Standard_NC24_v2 |24 |448 | 1344 | 4. | 32 |
| Standard_NC24r_v2 * |24 |1448 | 1344 | 4. | 32 |

GPU, 1 = один P100 карты.

* С поддержкой RDMA

## <a name="nd-instances"></a>Экземпляры ND

Виртуальные машины серии ND — это новое дополнение к семейству GPU, предназначены для рабочих нагрузок аналитики Активов и углубленного обучения. Они обеспечивают превосходную производительность для обучения и определения. Экземпляры ND берутся из [NVIDIA Tesla P40](http://images.nvidia.com/content/pdf/tesla/184427-Tesla-P40-Datasheet-NV-Final-Letter-Web.pdf) GPU. Эти экземпляры обеспечивают превосходную производительность для операций с плавающей запятой одиночной точности, для рабочих нагрузок AI Когнитивных набор средств Майкрософт, TensorFlow, Caffe и других платформ. В серии ND значительно увеличен объем памяти графического процессора (24 ГБ), что позволяет работать с моделями нейронных сетей гораздо большего размера. Последовательность NC серии ND предлагает конфигурации с вторичной малой задержкой, с высокой пропускной способностью сети через RDMA и чтобы можно было запустить задания крупномасштабных обучения охват многие графические процессоры сетевых средств InfiniBand.

> [!IMPORTANT]
> Для этого семейства размер квоты виртуальных процессоров (основной) в одном регионе в вашей подписке изначально установлено значение 0. [Запросить увеличение квоты виртуальных процессоров](../articles/azure-supportability/resource-manager-core-quotas-request.md) для этого семейства в [регион](https://azure.microsoft.com/regions/services/).
>

| Размер | vCPU | Память, ГиБ | Временное хранилище (SSD): ГиБ | Графический процессор | Максимальное количество дисков данных |
| --- | --- | --- | --- | --- | --- |
| Standard_ND6 |6 |112 | 336 | 1 | 12 |
| Standard_ND12 |12 |224 | 672 | 2 | 24 |
| Standard_ND24 |24 |448 | 1344 | 4. | 32 |
| Standard_ND24r * |24 |1448 | 1344 | 4. | 32 |

GPU, 1 = один P40 карты.

* С поддержкой RDMA

## <a name="nv-instances"></a>Экземпляры NV



Экземпляры NV берутся из [NVIDIA Tesla M60 ](http://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) графические процессоры и сетка NVIDIA технологии для рабочего стола accelerated приложения и виртуальные рабочие столы которой клиенты имеют возможность визуализировать свои данные или моделирование. С помощью экземпляров NV пользователи могут визуализировать свои рабочие процессы с большим объемом графики и получить расширенные возможности работы с графикой, а также запускать рабочие нагрузки одиночной точности, такие как кодирование и отрисовка. 

| Размер | vCPU | Память, ГиБ | Временное хранилище (SSD): ГиБ | Графический процессор | Максимальное количество дисков данных |
| --- | --- | --- | --- | --- | --- |
| Standard_NV6 |6 |56 |380 | 1 | 24 |
| Standard_NV12 |12 |112 |680 | 2 | 48 |
| Standard_NV24 |24 |224 |1440 | 4. | 64 |

1 графический процессор — половина графической карты M60.


