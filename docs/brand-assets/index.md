---
title: "Brand Assets"
---

<div>
  <ul style={{
    listStyle: 'none',
    paddingLeft: 0,
    margin: 0
  }}>
    {[
      { name: 'Rei Logo', link: "/assets/Rei_Logo.png", type: 'image' , size: '7 KB'},
      { name: 'Rei Logo Kit', link: "/assets/Rei_Logo_Kit.zip", type: 'archive', size: '232 KB' },
      { name: 'Rei Artworks', link: "/assets/Rei_Artworks.zip", type: 'archive', size: '4.9 MB' },
      { name: 'Chibi Rei', link: "/assets/Chibi_Rei.zip", type: 'archive', size: '47.8 MB' },
    ].map
    ((file, index) => (
      <li key={index} style={{
        display: 'flex',
        alignItems: 'center',
        gap: '15px',
        border: '1px solid grey',
        padding: '10px 20px',
        marginBottom: '20px',
      }}>
       <div style={{
          display: 'flex',
          flexDirection: 'column',
          alignItems: 'center',
          width: '80px'
        }}>
          <span>
            {file.type === 'archive' ? 
          <img
            src="/img/icn-zip.svg"
            style={{
              height: '30px',
            }}
          />:
          <img
            src="/img/icn-img.svg"
            style={{
              height: '30px',
            }}
          />}
          </span>
          <span style={{
            fontSize: '15px',
            color: '#000',
          }}>
            {file.size}
          </span>
        </div>

        {/* Vertical Pipe */}
        <div style={{
          height: '45px',
          width: '2px',
          backgroundColor: '#aaa'
        }}></div>

        <a
          href={file.link}
          download={file.name}
          style={{
            textDecoration: 'none',
            color: 'black',
            fontWeight: 'bold',
            flex: 1,
            ':hover': {
              textDecoration: 'underline'
            }
          }}
        >
          {file.name}
        </a>
      </li>
    ))}

  </ul>
</div>
