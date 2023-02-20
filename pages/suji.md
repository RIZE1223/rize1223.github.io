&nbsp;
<img src="../images/suji.png">







#　概要

## コンセプト
流れてくる数学に対して数字はめて道路を作る

## 制作意図
We,areTECH.C(卒業、進級制作展)2023出展用ゲームのチーム制作作品

## プラットフォーム
iPhone

## 使用言語
C#
Unity 2021.2.15f1

# 担当箇所

```cs
public class ScoreManager : SingletonMonoBehaviour<ScoreManager>
{
    public Text highScoreText; //ハイスコアを表示するText
    private int highScore; //ハイスコア用変数
    private string key = "HIGH SCORE"; //ハイスコアの保存先キー
    // Start is called before the first frame update
    void Start()
    {
        //highScore = PlayerPrefs.GetInt(key, 0);
        ////保存しておいたハイスコアをキーで呼び出し取得し保存されていなければ0になる
        //highScoreText.text = "HighScore: " + highScore.ToString();
        ////ハイスコアを表示
    }

    public void CompareHighScore(int score)
    {
        if (score > highScore)
        {

            highScore = score;
            //ハイスコア更新

            PlayerPrefs.SetInt(key, highScore);
            //ハイスコアを保存

            highScoreText.text = "HighScore: " + highScore.ToString();
            //ハイスコアを表示
        }

    }
}
```


```cs
public class HighScoreRank : MonoBehaviour
{

    [SerializeField, Header("Content")]
    private GameObject cntObj;

    private void Start()
    {
        LoadHighScore();
    }

    public void LoadHighScore()
    {
        Text[] rankArray = cntObj.GetComponentsInChildren<Text>();

        //順位1～30
        for (int i = 0; i < 5; i++)
        {
            int rank = i + 1;
            rankArray[i * 3].text = rank.ToString();
        }

        //データストア"HighScore"から検索
        NCMBQuery<NCMBObject> query = new NCMBQuery<NCMBObject>("Ranking");
        query.OrderByDescending("Score");

        query.FindAsync((List<NCMBObject> objList, NCMBException e) =>
        {
            //検索成功したら
            if (e == null)
            {
                for (int i = 0; i < 30; i++)
                {
                    if(objList.Count > i)
                    {
                        
                        

                        //ハイスコアを入力
                        rankArray[2 + (i * 3)].text = objList[i]["Score"].ToString() + "M";
                    }
                    else
                    {
                        break;
                    }
                }
            }
        });
    }

}
```
サーバーを使ってスコアランキングを１～５位まで表示するシステムを作りました。
