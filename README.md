# CoverFlow
![coverflow](https://user-images.githubusercontent.com/7846766/41909080-b84d1348-7963-11e8-82a1-81ac746f2e9b.gif)

# Requirements
Minimum OS 8.4 and later

# Manual Installation
You can directly add all the source files from CustomFlowLayout folder to your project.

# Introduce
CustomFlowLayout Controller is subclass of UICollectionViewFlowLayout with the custom animation and layout.


# Usage
Here is the instruction of how to use UICollectionViewFlowLayout.
1) Add native UICollectionView to the your view controller Xib.
2) Create your custom Collectionview cell.
3) Register your custom collection view cell in your View controller.
4) Set Custom Layout Flow of Collection view and alloc in your ViewController viewDidLoad() method and Explain in details Point 2 below.


1. Import CustomFlowLayout and PhotoCollectionViewCell module to your MIViewController class.
   
         #import "CustomFlowLayout.h"
         #import "PhotoCollectionViewCell.h"

         @interface MIViewController : UIViewController<FlowLayoutDelegate>
         {
             IBOutlet UICollectionView *collCoverFlow;
         }
     
 2. Add CustomFlowLayout and PhotoCollectionViewCell to MIViewController, then set delegate for it.
 
         - (void)initialize
            {
                self.title = @"Cover Flow";

                arrImages = [[NSMutableArray alloc] init];
                [self addItemInArray];

                //... Collection view registerNib

                [collCoverFlow registerNib:[UINib nibWithNibName:@"PhotoCollectionViewCell" bundle:nil] forCellWithReuseIdentifier:@"PhotoCollectionViewCell"];

                //... Set Custom Layout Flow of Collection view

                CustomFlowLayout *cLayout = [[CustomFlowLayout alloc] init];
                cLayout.delegate = self;
                [collCoverFlow setCollectionViewLayout:cLayout animated:NO];
            }

3. Add Iteams in array to MIViewController (Here we can add bundle images but you can use URL instead of this).

         // Add iteams
         - (void)addItemInArray
         {
             [arrImages addObjectsFromArray:@[@{@"image":@"1.jpg"},
                                              @{@"image":@"2.jpg"},
                                              @{@"image":@"3.jpg"},
                                              @{@"image":@"4.jpg"},
                                              @{@"image":@"5.jpg"}]];
         }
         
4. Add CollectionView Delegate to MIViewController

         #pragma mark
         #pragma mark - CollectionView Delegate

         - (NSInteger)collectionView:(UICollectionView *)collectionView numberOfItemsInSection:(NSInteger)section
         {
             //... total image + 1 for Last cell is Tap to Reload
             return arrImages.count + 1;
         }

         - (CGSize)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout*)collectionViewLayout sizeForItemAtIndexPath:(NSIndexPath *)indexPath
         {
             //... Propotional height and width of collection view cell according Device
             int size = CScreenWidth * 304 / 375;
             return CGSizeMake(size ,size);
         }

         - (UICollectionViewCell *)collectionView:(UICollectionView *)collectionView cellForItemAtIndexPath:(NSIndexPath *)indexPath
         {
             static NSString *identifier = @"PhotoCollectionViewCell";
             PhotoCollectionViewCell *cell = [collectionView dequeueReusableCellWithReuseIdentifier:identifier forIndexPath:indexPath];

             //... Configure cell

             cell.tag = indexPath.row;
             cell.imgLikeUnLike.hidden = true;

             if (indexPath.row == arrImages.count)
             {
                 //... Tap to Reload
                 cell.lblReload.hidden = NO;
                 cell.imageView.hidden = YES;
             }
             else
             {
                 //... Image cell
                 cell.lblReload.hidden = YES;
                 cell.imageView.hidden = NO;
                 NSDictionary *dictData = arrImages[indexPath.row];
                 cell.imageView.image = [UIImage imageNamed:[dictData valueForKey:@"image"]];
             }

             return cell;
         }

         - (void)collectionView:(UICollectionView *)collectionView didSelectItemAtIndexPath:(NSIndexPath *)indexPath
         {
             if (arrImages.count == 0)
             {
                 //... Last cell for Tap To Reload
                 //... When arrImage count is 0
                 [self addItemInArray];
                 [collCoverFlow reloadData];
             }
         }

5. Add Custom Flow layout Delegate to MIViewController.

            #pragma mark
            #pragma mark - Custom Flow layout Delegate

            - (BOOL)shouldCellMoveUpForIndexPath:(NSIndexPath *)indexpath
            {
                //... arrImage count greater than 0 return YES else return NO
                //... return YES that means Cell of indexpath is movable

                return arrImages.count != 0;
            }

            - (void)cellDidMovedUp:(UICollectionViewCell *)cell indexPath:(NSIndexPath *)indexPath
            {
                //...Remove item from Array and Reload Collection view
                [arrImages removeObjectAtIndex:indexPath.row];
                [collCoverFlow reloadData];
            }
            -(void)cellDidMovedRight:(UICollectionViewCell *)cell indexPath:(NSIndexPath *)indexPath
            {
                 PhotoCollectionViewCell *cellPhoto = (PhotoCollectionViewCell *) [collCoverFlow cellForItemAtIndexPath:indexPath];
                 cellPhoto.imgLikeUnLike.hidden = false;
                 cellPhoto.imgLikeUnLike.image = [UIImage imageNamed:@"unlike"];

            }

            -(void)cellDidMovedLeft:(UICollectionViewCell *)cell indexPath:(NSIndexPath *)indexPath
            {
                PhotoCollectionViewCell *cellPhoto = (PhotoCollectionViewCell *) [collCoverFlow cellForItemAtIndexPath:indexPath];
                cellPhoto.imgLikeUnLike.image = [UIImage imageNamed:@"like"];
                cellPhoto.imgLikeUnLike.hidden = false;
            }

            -(void)cellDidNotMoved:(UICollectionViewCell *)cell indexPath:(NSIndexPath *)indexPath
            {
                PhotoCollectionViewCell *cellPhoto = (PhotoCollectionViewCell *) [collCoverFlow cellForItemAtIndexPath:indexPath];
                cellPhoto.imgLikeUnLike.hidden = true;
            }
            
            
# Changelog
 ## Version 1.1
  -Adding Like and Dislike functionality on swipe left and right [#1](https://github.com/Mindinventory/CoverFlow/issues/1)
  

# LICENSE!

CoverFlow is [MIT-licensed](https://github.com/mindinventory/CoverFlow/blob/master/LICENSE).

## Let us know!
We’d be really happy if you send us links to your projects where you use our component. Just send an email to sales@mindinventory.com And do let us know if you have any questions or suggestion regarding our work.
