# jacky.github.io
https://posts.careerengine.us/p/60b8a7e576e9cb60031d29e4 參考
摘要:
隨著IT系統的快速發展，讓許多現代工廠裝設了多種感測器，而異常檢測技術也成為了日常營運中的關鍵任務，由於感測器都是實時監控，因此這類型的資料通常都是時間序列的資料，近年來，時間序列異常檢測一直是一個熱門的研究議題，眾所皆知，時間序列的時間依賴性是影響模型效能的一個重要因素，為此我們提出了一種STL-transformer，將時間序列透過STL分解將時間序列拆分成季節性，趨勢性和殘差，透過這三個分解區塊讓transformer模型能夠更專注於提取時間序列資料當中的特性，在三個真實世界的實驗資料集中，顯示了我們的方法在檢測準確性的優勢。

介紹:
時間序列異常檢測是一個熱門的研究議題，無論是在學術界或是工業界都有許多應用，異常檢測是指當資料出現與預期不同的變化時能夠識別出來，隨著物聯網及5G通訊的快速發展，許多工廠開始引入智慧化設備，這也讓工廠需要監控的感測器變得更多，已經無法再以人力去做識別，為了能夠在感測器發生異常之前更快速的發現，並採取預防措施，一個高效率的異常檢測技術可以讓工廠操作人員更便利的進行管理。
近十幾年來，感測器的增加除了提升異常檢測的難度，另一方面也代表著蒐集到的資料激增，這也讓異常檢測的難度越發困難，而在這方面的研究除了傳統的統計方法，還有監督式學習以及非監督式學習三類，統計方法中是利用資料中的平均數，變異數等概念去判別資料是否為異常，在監督式學習當中，時間序列異常檢測被視為一個二元分類的問題，透過機器學習模型或是神經網路模型去提取各種特徵，來獲取結果。雖然有大量的時間序列資料可以使用，但是資料異常出現的次數卻屈指可數，且異常需要由專家去做標註，這是一個耗時又昂貴的成本，因此第三類方法，非監督式學習成為了時間序列異常檢測中最多人參與的研究，他的核心思想是利用大量的常態資料訓練模型，雖然大部分的資料都是正常樣本，但是可能存在少數嚴重異常的樣本，這會讓模型更好的去檢測出這類型的資訊。
到目前為止已經有許多應用於多元時間序列異常檢測的方法，由於資料為度較高，因此在深度學習模型的效果通常比傳統的統計方法更好，在時間序列建模的時候，必須考慮時間特徵，RNN方法，可以允許序列的輸入藉此去學習時間特徵，但是其缺點在於無法保留長期的時間記憶，因此後來出現了LSTM，GRU等方法去改善這項缺點，但是其固有的限制導致無法獲取不同特徵之間的關係，為了同時捕捉時間長期依賴性以及特徵之間的關係，透過使用自注意力機制的Transformer成功學習的其順序表示。
然而在長期的環境下，時間序列異常檢測任務及具挑戰性，首先由於長期時間序列的時間模式非常複雜，所以提取時間依賴性很困難，因此[DATN]提出可以使用分解的方式來釐清複雜的時間模式，先將時間序列分解為趨勢性以及季節性，再利用快速傅立葉轉換來提取頻域的特徵，但是時間序列不僅僅只有趨勢性以及季節性，在[STL的論文]中提及殘差可能是影響時間序列最大的因素，基於上述訊息，我們提出了使用一種用於時間序列異常檢測的方法，STLtransformer，採用encoder-decoder架構，在encoder層我們將時間序列分解為趨勢性，季節性和殘差，並透過transformer-encoder去獲取三個組件的特徵。
我們的論文貢獻如下:
1.	在STL中分解時間序列，透過其LOESS的特性來平滑資料，對於不同的多元時間序列來說能夠更好的捕捉其特徵。
2.	在多個真實資料集的實驗結果表明，所提出的方法實現了最先進的效能。

相關文獻:
2.1	多元時間序列異常檢測
(a). predicted-based
(b). reconstructed-based
2.2	Transformer in 多元時間序列
Anomaly-transformer
DATN
TransAD

永豐:
from pptx.util import Pt
from pptx.enum.text import PP_ALIGN

def set_cell_font(cell, size, bold=False, color=RGBColor(0,0,0), align=PP_ALIGN.CENTER):
    """
    根据文本内容设置单元格中文为標楷體，英文为Arial。
    
    :param cell: 要设置的单元格
    :param size: 字体大小，单位Pt
    :param bold: 是否加粗
    :param color: 字体颜色，RGBColor对象
    :param align: 对齐方式，如PP_ALIGN.CENTER
    """
    # 定义两种字体
    chinese_font = '標楷體'
    english_font = 'Arial'

    # 获取单元格中的文本
    cell_text = cell.text_frame.text

    # 获取或创建段落
    tf = cell.text_frame
    p = tf.paragraphs[0] if tf.paragraphs else tf.add_paragraph()
    p.alignment = align
    p.clear()  # 清除现有内容

    # 初始化变量
    last_lang = None
    run = None

    # 遍历文本中的每个字符
    for char in cell_text:
        # 检查字符是中文还是其他
        is_chinese = '\u4e00' <= char <= '\u9fff'
        lang = 'chinese' if is_chinese else 'english'
        
        # 如果语言改变了，或者我们还没有创建任何文本运行
        if lang != last_lang or run is None:
            # 创建新的文本运行
            run = p.add_run()
            run.font.size = Pt(size)
            run.font.name = chinese_font if is_chinese else english_font
            run.font.bold = bold
            run.font.color.rgb = color

        # 将字符添加到当前文本运行
        run.text += char
        
        # 记录当前字符的语言
        last_lang = lang

# 使用示例
cell = table.cell(0, 0)
set_cell_font(cell, size=12, bold=True, color=RGBColor(255,0,0), align=PP_ALIGN.CENTER)


