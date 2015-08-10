#include <cv.h>
#include <highgui.h>
#include <stdio.h>
#include <iostream>
#include <sstream>
#include <fstream>
#include <unistd.h>

#define THRESHOLD 25
int mx1 = 0, my1 = 0, mx2 = 1279 , my2 = 719;
void onMouse (int event, int x, int y, int flags, void *param = NULL){
    static int count = 0;
    // (4)マウスイベントを取得
    switch (event) {
        case CV_EVENT_LBUTTONDOWN:
            if(count==0){
                mx1=x;
                my1=y;
                count++;
                printf("x1=%d,y1=%d\n", mx1, my1);
            }else{
                mx2=x;
                my2=y;
                count=0;
                printf("x2=%d,y2=%d\n", mx2, my2);
                //if(mx2<mx1) swap(&mx1,&mx2);
                //if(my2<my1) swap(&my1,&my2);
            }
            break;
    }
}

int main(int argc, char **argv) {
    using namespace cv;
    VideoCapture cap(0);  // VideoCapture(int device)
    if(!cap.isOpened()) return -1;
    
    Mat edges; //frameを処理した結果
    Mat pre; //frameの一個前の元データ
    double text;
    int count = 1;
    int flag = 0;
    int flag2 = 0;
    printf("bbbbbb");
    char code;
    unsigned int sleep(unsigned int seconds);
    //FILE *outputfile;
    cvNamedWindow("RokudenSystem", CV_WINDOW_AUTOSIZE);
    cvSetMouseCallback("RokudenSystem",onMouse);
    //cvSetMouseCallback ("OpenCV 2.0 Capture Test", on_mouse);
    for(;;) {
        Mat frame; // 今データ
        cap >> frame;  // virtual VideoCapture&  operator >> (Mat& image)
        cap >> edges;
        printf("%d,%d", frame.rows, frame.cols);
        if(flag == 1){
            // 全画素を赤くする
            for ( int y = my1 ; y < my2 ; ++y )
            {
                for( int x = mx1 ; x < mx2 ; ++x )
                    
                {
                    if (THRESHOLD >= abs(edges.at<Vec3b>(y, x)[0] - pre.at<Vec3b>(y, x)[0]) || THRESHOLD >= abs(edges.at<Vec3b>(y, x)[1] - pre.at<Vec3b>(y, x)[1]) || THRESHOLD >= abs(edges.at<Vec3b>(y, x)[2] - pre.at<Vec3b>(y, x)[2]) ){
                        
                    } else {
                        edges.at<Vec3b>(y, x)[0] = 0;
                        edges.at<Vec3b>(y, x)[1] = 0;
                        edges.at<Vec3b>(y, x)[2] = 0xff;
                        count++;
                    }
                }
            }
        } else{
            flag = 1;
            pre = frame;
        }
        text = count  * 100 / ((my2 - my1) * (mx2 - mx1)) ;
        std::ostringstream oss;
        oss << text;
        std::string abc = oss.str();
        putText(edges, abc, Point(50,50), FONT_HERSHEY_TRIPLEX, 1.5, Scalar(0,200,200), 2, CV_AA);
        imshow("RokudenSystem", edges);
        imwrite("/Users/Rokuden/Desktop/results/pre.png", pre);
        imwrite("/Users/Rokuden/Desktop/results/current.png", frame);
        imwrite("/Users/Rokuden/Desktop//results/result.png", edges);
        std::ofstream outputfile("/Users/Rokuden/Desktop/results/test.js");
        outputfile << "var t\n" << std::endl;
        outputfile << "t =" << std::endl;
        outputfile << text << std::endl;
        std::ofstream outputfilet("/Users/Rokuden/Desktop/results/test.html");
        outputfilet << "<link type='text/css' href='test.css' rel='stylesheet'/>" << std::endl;
        outputfilet << "<body>" << std::endl;
        outputfilet << "<p>" << std::endl;
        outputfilet << text << std::endl;
        outputfilet << "</p>" << std::endl;
        outputfilet << "</body>" << std::endl;

        outputfile.close();
        if(text >= 24) flag2++;
        if(text < 24) flag2 = 0;
        if(flag2 == 0 || flag2 > 5){
            code = waitKey(3000); //一時間
            if(code == 'q') break;
            if(code == 'r') pre = frame;
        }else if(flag2 == 5){ //メール送ってる
            system("echo 廊下に置かれている物が多くなっているようです。片付けてください。 | mail -s \"散らかり度が閾値を超えました\" rokuden@ht.sfc.keio.ac.jp");
            code = waitKey(3000); //こっちも10秒くらい？
            if(code == 'q') break;
            if(code == 'r') pre = frame;
        }else{
            code = waitKey(500); //繰り返し調べているところ 10秒くらい？
            if(code == 'q') break;
            if(code == 'r') pre = frame;
        }
        
        count = 0;
    }
    return 0;
}
