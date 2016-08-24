---
title: Android实现PopupWindow背景半透明（兼容方案）
date: 2015-12-27 11:37:42
tags: Android布局UI
category: Android笔记
---
大家想必对PopupWindow不会很陌生吧，我们在开发中经常会遇到要求使其背景半透明的需求，但网上的很多解决方案只能是在大部分机型上满足要求，像华为这样的机型就会发现我们原来设置的背景变暗效果的代码并没有起效果。
这里我贴出最终的兼容方案：

    View contentView;
        LayoutInflater mLayoutInflater = LayoutInflater.from(activity);
        contentView = mLayoutInflater.inflate(R.layout.layout_popupwindow,
                null);
        pop = new PopupWindow(contentView,
                ViewGroup.LayoutParams.MATCH_PARENT, (int) context.getResources().getDimension(R.dimen.y568));
        TextView tvTitle = (TextView) contentView.findViewById(R.id.text);
        tvTitle.setText(strTitle);
        ListView listView = (ListView) contentView.findViewById(R.id.list);
        // 产生背景变暗效果
        WindowManager.LayoutParams lp = activity.getWindow()
                .getAttributes();
        lp.alpha = 0.4f;
        activity.getWindow().addFlags(WindowManager.LayoutParams.FLAG_DIM_BEHIND);
        activity.getWindow().setAttributes(lp);
        pop.setTouchable(true);
        pop.setFocusable(true);
        pop.setBackgroundDrawable(new BitmapDrawable());
        pop.setOutsideTouchable(true);
        pop.showAtLocation(contentView, Gravity.BOTTOM, 0, 0);
        pop.update();
        pop.setOnDismissListener(new PopupWindow.OnDismissListener() {

            // 在dismiss中恢复透明度
            public void onDismiss() {
                WindowManager.LayoutParams lp = activity.getWindow()
                        .getAttributes();
                lp.alpha = 1f;
                activity.getWindow().addFlags(WindowManager.LayoutParams.FLAG_DIM_BEHIND);
                activity.getWindow().setAttributes(lp);
            }
        });
        listView.setOnItemClickListener(onItemClickListener);
        listView.setAdapter(adapter);
注：特别是下面几行代码

    // 产生背景变暗效果
        WindowManager.LayoutParams lp = activity.getWindow()
                .getAttributes();
        lp.alpha = 0.4f;
        activity.getWindow().addFlags(WindowManager.LayoutParams.FLAG_DIM_BEHIND);
        activity.getWindow().setAttributes(lp);
        pop.setTouchable(true);
        pop.setFocusable(true);
        pop.setBackgroundDrawable(new BitmapDrawable());
        pop.setOutsideTouchable(true);
        pop.showAtLocation(contentView, Gravity.BOTTOM, 0, 0);
        pop.update();
        pop.setOnDismissListener(new PopupWindow.OnDismissListener() {

            // 在dismiss中恢复透明度
            public void onDismiss() {
                WindowManager.LayoutParams lp = activity.getWindow()
                        .getAttributes();
                lp.alpha = 1f;
                  activity.getWindow().addFlags(WindowManager.LayoutParams.FLAG_DIM_BEHIND);
                activity.getWindow().setAttributes(lp);
            }
        });
网上很多方案都要求加下面这两行代码，但其实加上反而会影响华为这种机型的显示效果

    ColorDrawable dw = new ColorDrawable(-00000);
    popupWindow.setBackgroundDrawable(dw);
    ***

    ![FullStackEngineer的公众号，更多分享](https://github.com/logan62334/ImageArchive/raw/master/weixin/weixin.jpg)
