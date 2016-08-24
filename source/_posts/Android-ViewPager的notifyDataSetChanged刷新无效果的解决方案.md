---
title: '[Android] ViewPager的notifyDataSetChanged刷新无效果的解决方案'
date: 2016-06-05 16:11:55
tags: Android笔记
category: Android笔记
---
>最近在开发中遇到了一个问题：ViewPager设置的PagerAdapter调用notifyDataSetChanged()时界面无刷新以至于影响到功能的实现。不过有一个很傻的方法倒是可以解决就是给Viewpager重新设置一次适配器，下面我来分享一下如何优雅的解决这个问题吧。

大家进入ViewPager的源码可以看到下面的代码段：
```
 /**
     * Set a PagerAdapter that will supply views for this pager as needed.
     *
     * @param adapter Adapter to use
     */
    public void setAdapter(PagerAdapter adapter) {
        ......

        if (mAdapter != null) {
            if (mObserver == null) {
                mObserver = new PagerObserver();
            }
            mAdapter.setViewPagerObserver(mObserver);
            ......
        }

        ......
    }

    private class PagerObserver extends DataSetObserver {
        @Override
        public void onChanged() {
            dataSetChanged();
        }
        @Override
        public void onInvalidated() {
            dataSetChanged();
        }
    }

    void dataSetChanged() {
       ......

        for (int i = 0; i < mItems.size(); i++) {
            final ItemInfo ii = mItems.get(i);
            final int newPos = mAdapter.getItemPosition(ii.object);

            if (newPos == PagerAdapter.POSITION_UNCHANGED) {
                continue;
            }

            if (newPos == PagerAdapter.POSITION_NONE) {
                mItems.remove(i);
                i--;

                if (!isUpdating) {
                    mAdapter.startUpdate(this);
                    isUpdating = true;
                }

                mAdapter.destroyItem(this, ii.position, ii.object);
                needPopulate = true;

                if (mCurItem == ii.position) {
                    // Keep the current item in the valid range
                    newCurrItem = Math.max(0, Math.min(mCurItem, adapterCount - 1));
                    needPopulate = true;
                }
                continue;
            }

            ......

        }

    }
```
意思是如果item的位置如果没有发生变化，则返回POSITION_UNCHANGED；如果item的位置已经不存在了，则回了POSITION_NONE。
#### 解决方案
```
private class SetDialogAdapter extends PagerAdapter {

    private int mChildCount = 0;

    @Override public void notifyDataSetChanged() {
      mChildCount = getCount();
      super.notifyDataSetChanged();
    }

    @Override public int getItemPosition(Object object) {
      if (mChildCount > 0) {
        mChildCount--;
        return POSITION_NONE;
      }
      return super.getItemPosition(object);
    }
}
```
我们覆盖getItemPosition()方法，当调用notifyDataSetChanged时，让getItemPosition方法强行返回POSITION_NONE，从而让ViewPager重绘所有item。
***
![FullStackEngineer的公众号，更多分享](https://github.com/logan62334/ImageArchive/raw/master/weixin/weixin.jpg)
