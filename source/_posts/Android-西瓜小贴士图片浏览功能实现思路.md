---
title: 西瓜小贴士图片浏览功能实现思路
date: 2016-03-07 23:04:02
tags: Android 图片浏览
category: Android笔记
---
最近翻到前段时间的一版设计稿，如下图这样的一个效果，当时都已经实现了但后来由于需求变更所以……没能让它和广大用户见面，但感觉这种效果还是不错的所以拿出来分享一下。

![](https://github.com/logan62334/ImageArchive/raw/master/Android/4.jpg)

#### 功能需求
1、右上角是页数指示器
2、左右两个操作按钮要求触摸时时消失
3、可以左右滑动
#### 实现思路
1、图片预存在本地通过Fresco来加载
2、滑动效果通过ViewPager+fragment实现

MainActivity.java

```
public class MainActivity extends AppCompatActivity {

    private static final Integer[] IMAGES = new Integer[] {
            R.mipmap.tip1,R.mipmap.tip2,R.mipmap.tip3,R.mipmap.tip4,R.mipmap.tip5,R.mipmap.tip6,R.mipmap.tip7,R.mipmap.tip8
    };

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Fresco.initialize(this);

        final ViewPager pager = (ViewPager) findViewById(R.id.pager);
        PageIndicator mPageIndicator= (PageIndicator) findViewById(R.id.indicator);
        ImageView ivLeft= (ImageView) findViewById(R.id.ivLeft);
        ImageView ivRight= (ImageView) findViewById(R.id.ivRight);

        ivLeft.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                pager.setCurrentItem(pager.getCurrentItem()-1);
            }
        });
        ivRight.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                pager.setCurrentItem(pager.getCurrentItem()+1);
            }
        });


        SlidePagerAdapter pagerAdapter =
                new SlidePagerAdapter(getSupportFragmentManager());

        // set pictures
        pagerAdapter.addAll(Arrays.asList(IMAGES));

        pager.setAdapter(pagerAdapter);
        pager.addOnPageChangeListener(new ViewPager.OnPageChangeListener() {
            @Override
            public void onPageScrolled(int position, float positionOffset, int positionOffsetPixels) {

            }

            @Override
            public void onPageSelected(int position) {

            }

            @Override
            public void onPageScrollStateChanged(int state) {

            }
        });

        mPageIndicator.setViewPager(pager);
    }
```
SlidePageFragment.java

```
public class SlidePageFragment extends Fragment {
    private static final String PIC_URL = "slidepagefragment.picurl";

    public static SlidePageFragment newInstance(@NonNull final int picUrl) {
        Bundle arguments = new Bundle();
        arguments.putInt(PIC_URL, picUrl);

        SlidePageFragment fragment = new SlidePageFragment();
        fragment.setArguments(arguments);

        return fragment;
    }

    @Override
    public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container,
                             @Nullable Bundle savedInstanceState) {
        View rootView = inflater.inflate(R.layout.fragment_slide_page, container, false);

        SimpleDraweeView view = (SimpleDraweeView) rootView.findViewById(R.id.pic);

        Bundle arguments = getArguments();
        if (arguments != null) {
            int url = arguments.getInt(PIC_URL);
            view.setBackgroundResource(url);
        }

        return rootView;
    }
}
```
SlidePagerAdapter.java

```
public class SlidePagerAdapter extends FragmentStatePagerAdapter {
    private List<Integer> picList = new ArrayList<>();

    public SlidePagerAdapter(FragmentManager fm) {
        super(fm);
    }

    @Override
    public Fragment getItem(int i) {
        return SlidePageFragment.newInstance(picList.get(i));
    }

    @Override
    public int getCount() {
        return picList.size();
    }

    public void addAll(List<Integer> picList) {
        this.picList = picList;
    }
}
```
详细文件请访问：https://github.com/logan62334/Gallery
***

![FullStackEngineer的公众号，更多分享](https://github.com/logan62334/ImageArchive/raw/master/weixin/weixin.jpg)
