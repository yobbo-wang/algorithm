Geo哈希算法,用于将坐标哈希成32位字符串
说明：
1<=precision<=12



public class Test {

    public static void main(String[] args) {
        // 11111 = 31  [31]=w  32个数
        String encode = new TestController().encode(39.92816697, 116.38954991, 12);
        System.out.println(encode);
    }

    private static class Interval{
        enum Type{LONGITUDE, LATITUDE};
        double lower;
        double higher;

        public Interval(Type s){
            switch (s){
                case LONGITUDE:
                    this.lower = -180;
                    this.higher = 180;
                    break;
                case LATITUDE:
                    this.lower = -90;
                    this.higher = 90;
                    break;
            }
        }

        public int narrow(double target){
            double mid = lower + (higher - lower) /2;
            int ans;
            if(target <= mid){
                this.higher = mid;
                ans = 0;
            }
            else {
                this.lower = mid;
                ans = 1;
            }
            return ans;
        }
    }

    private static final String BASE32 = "0123456789bcdefghjkmnpqrstuvwxyz";
    private String bings(String s){
        int c = 0;
        String we = "";
        while (c < s.length()){
            String r = s.substring(c, c + 5);
            int i = Integer.parseInt(r, 2);
            we += BASE32.charAt(i);
            c += 5;
        }

        return we;
    }


    public String encode(double lat, double lon, int precision){
        int num = precision * 5;
        Interval[] intervals = new Interval[2];
        intervals[0] = new Interval(Interval.Type.LONGITUDE);
        intervals[1] = new Interval(Interval.Type.LATITUDE);
        double[] target = {lon, lat};
        String serial = "";
        for(int i=0;i<num;i++){
            Interval temp = intervals[i % 2];
            serial += temp.narrow(target[i%2]);
        }

        return bings(serial);
    }
}
