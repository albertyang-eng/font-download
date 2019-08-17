# font-download
How to download fonts to get callbacks
export  const addFontface = (fontfamily: any, fontType: any, callback?: Function) => {
    if (fontType && fontType !== '0') {
        // 如果没下载过字体
        if (!isAddfont(fontfamily)) {
            FontManageLockUtils.fontStatus = '下载中';
            var bitterFontFace = new (window as any).FontFace(`${fontfamily}`, `url(${Api.FONT_SOURCE_URL}${fontType}.ttf)`);
            // (document as any).fonts.add(bitterFontFace);
            bitterFontFace.loaded.then((fontface) => callback(fontface , bitterFontFace));
            bitterFontFace.load();
        } else {
            // console.log('已经下载过字体' + bitterFontFace);
            FontManageLockUtils.fontStatus = '已完成';
            callback('', bitterFontFace);
        }
    }
};
export const isAddfont = (fontfamily: any) => {
    let isAdd = false;
    let fontObject = (document as any).fonts;
    let it = fontObject.values();
    let curr = it.next();
    while (curr && curr.value) {
        if (curr.value.family == fontfamily) {
            isAdd = true;
            // console.error('curr', curr);
            break;
        }
        curr = it.next();
    }
    return isAdd;
};
downloadFont = () => {
        // this.props.fontfamily 表示当前选择字体的 key('1' - '8')
        let fontFamily = getFontFamily(this.state.fontfamily);
        if (!isAddfont(fontFamily)) {
            message.info('字体下载中');
            addFontface(fontFamily, this.state.fontfamily, (fontface, bitterFontFace) => {
                // console.log('当前下载字体', fontface.family , '字体对象' + bitterFontFace);
                if (fontface != '') {
                    // 加载字体完成定义字体
                    (document as any).fonts.add(bitterFontFace);
                    this.props.saveFontSettingKey(this.state.fontfamily);
                    message.success('字体下载完成');
                    this.changeStatus();
                } else {
                    // message.info('所选字体已经下载过');
                }
            });
        }
    }
