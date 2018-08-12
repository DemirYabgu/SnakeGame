# SnakeGame
Kodladığım ilk projem.

Ana Pencere

public final class AnaPencere extends JFrame {
    
    
    private final int mGenislik;
    private int mYukseklik = 600;
   
    private static AnaPencere mPencere =null;
    
    private AnaPencere()
    { 
        this.mGenislik = 600;
        setDefaultCloseOperation(EXIT_ON_CLOSE);
        setDimension(mGenislik, mYukseklik );
        setResizable(false);
        
      yılan Y = new yılan();
      
      add(Y);
      
    }

    public static AnaPencere PencereGetir()
     {       
     
         if (mPencere==null)
            mPencere = new AnaPencere();
        
         return mPencere;
         
     }
       public void setDimension(int Genislik,int Yukseklik)
      {
         Dimension Dim = Toolkit.getDefaultToolkit().getScreenSize();
         
         int PosX = 0;
         int PosY = 0;
         
         if(mGenislik+100> Dim.width)
             Genislik = Dim.width-100;
         if(mYukseklik+100> Dim.height)
             mYukseklik = Dim.height-100;
         
         PosX = (Dim.width-mGenislik)/2;
         PosY = (Dim.height-mYukseklik)/2;
         
         setBounds(PosX, PosY, mGenislik, mYukseklik);
     }
         
         
                
    }

     
Yılanı Oluşturan Kutular

public class kutu extends JLabel{
    public int mGenislik = 20;   
    public int mYon      = YON.SAG;
   kutu()
   {        
       setBounds(100, 100, mGenislik, mGenislik);
   }
    @Override
   public void paint(Graphics g)
   {
        
        Graphics2D g2 = (Graphics2D)g;
     
        Rectangle2D rect = new Rectangle2D.Double(1, 1, getWidth()-2, getHeight()-2);
        
        g2.setColor(Color.black);
        
        g2.setStroke(new BasicStroke(2));
        
        g2.draw(rect); 
        
        g2.setColor(Color.red);
        
        g2.fill(rect);  
   }
   public void SolaGit()
   {
       int PosX = getX();
       int PosY = getY();
             
       PosX-=mGenislik;
       setBounds(PosX,PosY,mGenislik,mGenislik);
         
   }
   public void SagaGit()
   {
        int PosX = getX();
       int PosY = getY();
             
       PosX+=mGenislik;
       setBounds(PosX,PosY,mGenislik,mGenislik);
         
   } 
      public void YukariGit()
   {
        int PosX = getX();
       int PosY = getY();
             
       PosY-=mGenislik;
       setBounds(PosX,PosY,mGenislik,mGenislik);
         
   } 
      public void AsagiGit()
   {
        int PosX = getX();
       int PosY = getY();
             
       PosY+=mGenislik;
       setBounds(PosX,PosY,mGenislik,mGenislik);
         
   } 
      public kutu KutuOlustur()
      {
          kutu K = new kutu();
          
          int X = getX();
          int Y = getY();
          
          K.setBounds(X,Y,mGenislik,mGenislik);
          
          K.mYon = -mYon;
          
          K.Hareket();
          
          K.mYon = mYon;
          
          
          return K;
      }
      public void Hareket()
      {
          if (mYon == YON.SOL)
              SolaGit();
          else if(mYon == YON.SAG)
              SagaGit();
          else if(mYon == YON.YUKARI)
              YukariGit();
          else if(mYon == YON.ASAGI) 
              AsagiGit();
      }
}

Yön Interface

public interface YON 
{
    public static final int SOL = 1; 
    public static final int ASAGI = -2;
    public static final int YUKARI = 2;
    public static final int SAG = -1;
    
}

Yılanın Kodları

public class yılan extends JLabel {

    public kutu mHead = new kutu();
    
    public Timer mTimer=null;
    
    public Yem mYem = new Yem();
    
    public Random  mRandom = null;
    
    public ArrayList<kutu> Liste = new ArrayList<kutu>();
            
    @Override
    
    public void paint(Graphics g) {
        super.paint(g);
        
        int W = getWidth();
        
        System.out.println(W);
        Graphics2D g2 = (Graphics2D)g;
     
        Rectangle2D rect = new Rectangle2D.Double(5, 5, getWidth()-10, getHeight()-10);
        
        g2.setStroke(new BasicStroke(10));
        
        g2.draw(rect);
   }
    yılan()
    {
        
        mRandom = new Random(System.currentTimeMillis());
        addKeyListener(new klavyekontrol());
        setFocusable(true);
        
        mTimer = new Timer(100,new timerkontrol());
        mTimer.start();
        
        Liste.add(mHead);
        for(int i=1;i<10;i++)
        {
           KuyrukEkle();
        }
     
        add(mYem);
        add(mHead);
    }
    public void KuyrukEkle()
    
    {
        kutu K = Liste.get(Liste.size()-1).KutuOlustur();
            
        Liste.add(K);
        add(K);
    }
     public void YemEkle()
    {        
          int Width = getWidth()-30-mYem.mGenislik;
          int Height = getHeight()-30-mYem.mGenislik;
          
          int PosX = 10+Math.abs(mRandom.nextInt())%Width;
          int PosY = 10+Math.abs(mRandom.nextInt())%Height;
         
          PosX = PosX - PosX%20;
          PosY = PosY - PosY%20;
          
          mYem.setPosition(PosX, PosY);
    }
    
    public void HepsiniYurut()
    {
        for(int i=Liste.size()-1;i>0;i--)
        {
            kutu Onceki = Liste.get(i-1);
            kutu Sonraki = Liste.get(i);
            
            
            Liste.get(i).Hareket();
            
            Sonraki.mYon = Onceki.mYon;
        }    
        mHead.Hareket();
    
    if((mYem.getX()==mHead.getX())&&(mYem.getY()==mHead.getY()))
            {
               KuyrukEkle();
               YemEkle();
            }
   
        }
            public boolean CarpismaVarmi()
        {
            int Kalem =  10;
            
            int Genislik = getWidth();
            int Yukseklik = getHeight();

            if(mHead.getX()<=10)
               return true;
   
      }
    class klavyekontrol implements KeyListener 
    {

        @Override
        public void keyTyped(KeyEvent e) {
       }

        @Override
        public void keyPressed(KeyEvent e) {
            if(e.getKeyCode()  == KeyEvent.VK_LEFT)
            {
               if(mHead.mYon!=YON.SAG)
                mHead.mYon = YON.SOL;
            }    
            if(e.getKeyCode()  == KeyEvent.VK_RIGHT)
            {
               if(mHead.mYon!=YON.SOL)
                mHead.mYon = YON.SAG;
            }
            if(e.getKeyCode()  == KeyEvent.VK_UP)
            {
               if(mHead.mYon!=YON.ASAGI)
                mHead.mYon = YON.YUKARI;
            }
            if(e.getKeyCode()  == KeyEvent.VK_DOWN)
            {
                mHead.mYon = YON.ASAGI; 
            }
             
        }

        @Override
        public void keyReleased(KeyEvent e) {
        }
        
    }
    class timerkontrol implements ActionListener
    {   

        @Override
        public void actionPerformed(ActionEvent e) {

        HepsiniYurut(); }
          




         
         
            
            
          
       
  
