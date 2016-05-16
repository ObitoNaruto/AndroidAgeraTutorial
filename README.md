# AndroidAgeraTutorial
Android Agera Example.

# Change Text Color
## Click Button Send Update Event

```
    //onClick Observable
    mObservable = new OnClickObservable() {
        @Override
        public void onClick(View view) {
            dispatchUpdate();
        }
    };
    //when click button, then dispatchUpdate()
    mBinding.setObservable(mObservable);
    android:onClick="@{observable::onClick}"
```

## Repository

```
    //text color Supplier
    Supplier<Integer> supplier = new Supplier<Integer>() {
        @NonNull
        @Override
        public Integer get() {
            return MockRandomData.getRandomColor();
        }
    };
    //
    mRepository = Repositories.repositoryWithInitialValue(0)
            .observe(mObservable)
            .onUpdatesPerLoop()
            .thenGetFrom(supplier)
            .compile();

    @Override
    protected void onResume() {
        super.onResume();
        mRepository.addUpdatable(this);
    }

    @Override
    protected void onPause() {
        super.onPause();
        mRepository.removeUpdatable(this);
    }
```

## setTxtColor

```
    @Override
    public void update() {
        mBinding.setTxtColor(mRepository.get());
    }
```

# Load Image By Picasso
## Repository
```

    Supplier<String> imageUriSupplier = new Supplier<String>() {
        @NonNull
        @Override
        public String get() {
            return MockRandomData.getRandomImage();
        }
    };

    mRepository = Repositories.repositoryWithInitialValue(Result.<Bitmap>absent())
            .observe(mObservable)
            .onUpdatesPerLoop()
            .getFrom(imageUriSupplier)//image uri
            .goTo(networkExecutor)
            .thenTransform(new Function<String, Result<Bitmap>>() {
                @NonNull
                @Override
                public Result<Bitmap> apply(@NonNull String input) {
                    return new ImageSupplier(input).get();//image bitmap
                }
            })
            .compile();
```

<br/>
